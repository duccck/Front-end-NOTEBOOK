## client
|API|定义|包含|适用对象|
|--|--|--|--|
|clientWidth|width|padding|element|
|clientHeight||||
|clientTop|top border width|||
|clientLeft||||
|--|--|--|--|
|clientX|returns the **horizontal coordinate (according to the window)** of the mouse pointer when a mouse event was triggered||usually with **mouse event**|
|clientY||||
## offset
|API|定义|包含|适用对象|
|--|--|--|--|
|offsetWidth|viewable width|padding / border / scrollbar|element|
|offsetHeight||||
|--|--|--|--|
|offsetLeft|left position (in pixels) relative to the left side the offsetParent element (the nearest ancestor that has a position other than static)|self's margin|element|
|offsetTop||||
|--|--|--|--|
|offsetX|the x-coordinate of the mouse pointer, relative to the target element||usually with **mouse event**|
|offsetY||||
## scroll
|API|定义|包含|适用对象|
|--|--|--|--|
|scrollWidth|entire width (in pixels)|padding|element|
|scrollHeight|entire height|||
|--|--|--|--|
|scrollLeft|sets or returns the number of pixels an element's content is scrolled horizontally||element (usually with **scroll event**)|
|scrollTop||||
|--|--|--|--|
|scrollX|returns the pixels the current document has been scrolled **from the upper left corner of the window** horizontally||window|
|scrollY||||
## inner & outer
|API|定义|包含|适用对象|
|--|--|--|--|
|innerWidth|width of a **window's content area**||window|
|innerHeight||||
|outerWidth|width of the browser window|toolbars / scrollbars||
|outerHeight||||
## page
|API|定义|包含|适用对象|
|--|--|--|--|
|pageXOffset|same with scrollX|||
|pageYOffset||||
|--|--|--|--|
|pageX|the horizontal coordinate (according to the web page, in pixels) of the mouse pointer when a mouse event was triggered||usually with **mouse event**|
|pageY||||
## screen
|API|定义|包含|适用对象|
|--|--|--|--|
|screenX|the horizontal coordinate (according to the users computer screen, in pixels) of the mouse pointer when an event was triggered||usually with **mouse event**|
|screenY||||
|--|--|--|--|
|screenX|the x (horizontal) coordinates of the window **relative to the screen**||window|
|screenY||||
|screenLeft|same with screenX|||
|screenTop||||
## getBoundingClientRect()
|API|定义|包含|适用对象|
|--|--|--|--|
||The `left / x`, `top / y`, `right`, `bottom`, `width`, `height` properties describe the position **according to viewport** and size of the overall rectangle in pixels|padding / border width|element|
