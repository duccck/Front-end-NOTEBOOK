## inner & outerHTML or Text
|API|定义|包含|适用对象|
|--|--|--|--|
|innerHTML|sets or returns the HTML content|HTML tag in element|element|
|innerText|sets or returns the text content of the specified node|element's descendants' text contents||
|outerHTML|sets or returns the HTML element and all it's content, including the start tag, it's attributes, and the end tag|entire element||
|outerText|sets or returns the text content of the specified node (same with innerText). **There is an important difference when setting an element's outerText, because the element itself is removed**|||
