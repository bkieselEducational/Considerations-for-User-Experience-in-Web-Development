# For A Good User Experience, Call...


## Consideration #1: Shifting Page Content
A common issue seen in the apps made by new web devs is the LACK of consideration for page content moving around when being acted upon by the user.
This is most commonly observed in a couple of areas:
1. Rendering validation error messages on a form.
2. adding or removing borders from elements that have been selected or unselected.

Let's look at some easy fixes for these issues!

### Rendering Validation Error Messages on a Form

Generally speaking, there are 2 methods for rendering validation error messages on a form. Either 1) Render a parent element at the top of the form, inside of which, you will map over an array of pertinent error messages OR (my personal preference) 2) rendering an error message immediately above or below the concerned input field.
In both cases, when error messages are rendered on the form, the form grows either in size (height: fit-content) and/or elements within the form are moved from their current positions to make room for the error messages. AND in both cases, this issue can be remedied by acoounting for that space BEFORE the messages are rendered. In case #1, where we render messages at the top of the form, we must first figure out how much space the messages will take when ALL of the error messages are rendered at once. With this value in hand, we can simply set the CSS property 'min-height' on that parent element, so that it is always taking up that much space on the form regardless of error messages being rendered or not. The solution to case #2 is essentially the same. Only in this case, we are really just getting the height of a SINGLE error message. Depending on your intentions for the site layout and whatever particular use case you might have, this value can be a fixed number of pixels or a % of the containing element. To accomodate all of the various screen sizes available (mobile, oh my!!!), it is recommended to have a MINIMUM and MAXIMUM size for most elements. The minimum and maximum values should be related to a minimum and maximum content width. Widths that fall between these two bounds, should be adjusted DYNAIMCALLY!!
