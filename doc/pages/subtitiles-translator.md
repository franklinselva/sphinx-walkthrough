# subtitiles-translator

## subtitles translator disney

<!-- MARKDOWN-AUTO-DOCS:START (CODE:src=../../python/subtitiles-translator/subtitles-translator-disney.py) -->
<!-- The below code snippet is automatically added from ../../python/subtitiles-translator/subtitles-translator-disney.py -->
```py
# simple application for translate all Disney subtitles (JS filter "seg") 
# from DEutsch to RUssian
# 
# file example
# 00:01:06.691 --> 00:01:09.069 line:14.96%,start
# 20 Jahre zuvor
#
# 00:01:11.196 --> 00:01:15.992
# Was machen wir nur hier? Kannst du sie
# nicht einfach dazu bringen, Ja zu sagen?
# 
# 00:01:16.076 --> 00:01:18.495 line:85.04%,end
# Ich kÃ¶nnte, aber das ist nicht meine Art.

# usage example
# cat subtitles.txt | python3 subtitles-translator.py<F12>


import sys
import subprocess
from time import sleep

def clear_binary_line(b_line):
    return b_line.decode('utf-8').rstrip()


buffer = ""

for line in sys.stdin:
    prepared_line = line.strip()
    if len(prepared_line)==0:
        continue
    
    remove_marker = prepared_line.find("-->")    
    if remove_marker > 0:
        sleep(5)
        translation = subprocess.check_output(["trans", "--no-warn", "-source", "de","-target","ru", "-brief", buffer])
        print(buffer, "   ")
        print("```",clear_binary_line(translation),"```")
        print(line[0:remove_marker]+"  \n---")
        buffer = ""
    else:        
        buffer = buffer + " " + prepared_line
```
<!-- MARKDOWN-AUTO-DOCS:END -->



## ttml translator

<!-- MARKDOWN-AUTO-DOCS:START (CODE:src=../../python/subtitiles-translator/ttml-translator.py) -->
<!-- The below code snippet is automatically added from ../../python/subtitiles-translator/ttml-translator.py -->
```py
#!/usr/bin/env python3
# ./ttml-translator.py /path/to/times/text/markup/language
# <tt:body region="AmazonDefaultRegion">
#        <tt:div>
#            <tt:p begin="00:01:23.334" end="00:01:25.044">- Ja?<tt:br/>- Du wurdest enttarnt.</tt:p>
#            <tt:p begin="00:01:25.127" end="00:01:27.838">- Fahr sofort zur Botschaft.<tt:br/>- Okay.</tt:p>

import subprocess
import sys
from functools import reduce
from time import sleep
from typing import List
from xml.dom.minidom import parse, parseString, Element, Document, Node


def child_nodes_to_text(nodes):
    return reduce(lambda x, y: x + "\n" + y,
                  map(lambda _: _.nodeValue, filter(lambda _: _.nodeType == Node.TEXT_NODE, nodes)))


def translate_from_german(text):
    translation = subprocess.check_output(["trans", "--no-warn", "-source", "de", "-target", "ru", "-brief", text])
    sleep(3)
    return translation.decode('utf-8').rstrip()


if __name__ == "__main__":
    if len(sys.argv) < 1:
        print("can't find path to TimedTextMarkupLanguage file as input argument ")
        sys.exit(1)
    path_to_file = sys.argv[1]
    dom: Document = parse(path_to_file)
    components: List[Element] = [each_element for each_element in dom.getElementsByTagName("tt:p")]
    for each_component in components:
        print("===", each_component.getAttribute("begin"), each_component.getAttribute("end"))
        text = child_nodes_to_text(each_component.childNodes)
        #print(text, "\n", translate_from_german(text))
        print(text)
```
<!-- MARKDOWN-AUTO-DOCS:END -->



## ttml translator2

<!-- MARKDOWN-AUTO-DOCS:START (CODE:src=../../python/subtitiles-translator/ttml-translator2.py) -->
<!-- The below code snippet is automatically added from ../../python/subtitiles-translator/ttml-translator2.py -->
```py
#!/usr/bin/env python3
# ./ttml-translator.py /path/to/times/text/markup/language
# <tt:body region="AmazonDefaultRegion">
#        <tt:div>
#            <tt:p begin="00:01:23.334" end="00:01:25.044">- Ja?<tt:br/>- Du wurdest enttarnt.</tt:p>
#            <tt:p begin="00:01:25.127" end="00:01:27.838">- Fahr sofort zur Botschaft.<tt:br/>- Okay.</tt:p>

import subprocess
import sys
from functools import reduce
from time import sleep
from typing import List
from xml.dom.minidom import parse, parseString, Element, Document, Node


def retrieve_text(node: Element) -> str:
    if node.hasChildNodes():
        elements = list(
            filter(lambda _: _.nodeType == Node.TEXT_NODE or (_.nodeType == Node.ELEMENT_NODE and _.tagName == "span"),
                   node.childNodes))
        mapping = map(lambda x: x.nodeValue if x.nodeType == Node.TEXT_NODE else x.firstChild.nodeValue, elements)
        return reduce(lambda x, y: x + "\n" + y, mapping)
    else:
        return str(node)


def translate_from_german(text):
    translation = subprocess.check_output(["trans", "--no-warn", "-source", "de", "-target", "ru", "-brief", text])
    sleep(3)
    return translation.decode('utf-8').rstrip()


if __name__ == "__main__":
    if len(sys.argv) < 1:
        print("can't find path to TimedTextMarkupLanguage file as input argument ")
        sys.exit(1)
    path_to_file = sys.argv[1]
    dom: Document = parse(path_to_file)
    components: List[Element] = [each_element for each_element in dom.getElementsByTagName("p")]
    for each_component in components:
        print("===", each_component.getAttribute("begin"), each_component.getAttribute("end"))
        text = retrieve_text(each_component)
        # print(text, "\n", translate_from_german(text))
        print(text)
```
<!-- MARKDOWN-AUTO-DOCS:END -->


