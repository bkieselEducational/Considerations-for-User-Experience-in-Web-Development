# For A Good User Experience, Call (555-CSS-GOOD)


## Consideration #1: Shifting Page Content
A common issue seen in the apps made by new web devs is the LACK of consideration for page content moving around when being acted upon by the user.
This is most commonly observed in a couple of areas:
1. Rendering validation error messages on a form.
2. Adding or removing borders from elements that have been selected or unselected.

Generally speaking, there are 2 methods for rendering validation error messages on a form. Either 1) Render a parent element at the top of the form, inside of which, you will map over an array of pertinent error messages OR (my personal preference) 2) rendering an error message immediately above or below the concerned input field.
In both cases, when error messages are rendered on the form, the form grows either in size (height: fit-content) and/or elements within the form are moved from their current positions to make room for the error messages. AND in both cases, this issue can be remedied by acoounting for that space BEFORE the messages are rendered. In case #1, where we render messages at the top of the form, we must first figure out how much space the messages will take when ALL of the error messages are rendered at once. With this value in hand, we can simply set the CSS property 'min-height' on that parent element, so that it is always taking up that much space on the form regardless of error messages being rendered or not. The solution to case #2 is essentially the same. Only in this case, we are really just getting the height of a SINGLE error message. Depending on your intentions for the site layout and whatever particular use case you might have, this value can be a fixed number of pixels or a % of the containing element. To accomodate all of the various screen sizes available (mobile, oh my!!!), it is recommended to have a MINIMUM and MAXIMUM size for most elements. The minimum and maximum values should be related to a minimum and maximum content width. Widths that fall between these two bounds, should be adjusted DYNAMICALLY!! HINT: Either by djusting your browser manually or by using Chrome's builtin screen sizes for popular mobile devices, you can figure out what minmum and maximum sizes fit your application. Also note that aside from adjusting the size of elements, you'll also want to adjust the size of the text on your page! Let's explore some generic solutions for these issues! 

### Rendering Validation Error Messages on a Form

```
{/* YOUR FORM COMPONENT (Render Errors at Top) */}

<div className="form-errors">
  {yourValidationErrors.map((err, idx) => (
    <div key={idx}>{err}</div>
  ))}
</div>

{/* YOUR FORM COMPONENT (Render Error below input) */}

<input type="email" className="form-input"/>
<div className="form-errors">{errors.email}</div>

/* YOUR CSS FILE FOR FORM COMPONENT */

.form-errors {
  min-height: THE VALUE YOU'VE DECIDED ON
  ...
  ...
}

```

### Adding and Removing Borders from Elements

```
/*** Your CSS File ***/

:root {
  /* Site Colors */
  --background-color: <YOUR CHOSEN COLOR>
  ...
  ...
}

.class-name {
  border: 1px solid var(--background-color);
}

.class-name:hover {
  border-color: <YOUR DESIRED HOVER COLOR>
}

#id-name {
  border: 1px solid var(--background-color);
}

#id-name:hover {
  border-color: <YOUR DESIRED HOVER COLOR>
}

```

## Consideration #2: Making Elements and Text Responsive

<img width="713" alt="responsive" src="https://github.com/bkieselEducational/Considerations-for-User-Experience-in-Web-Development/assets/131717897/b56c4724-6e47-4486-8cc9-a6393d1522d0">

## Consideration #3: Prevent Bad Actors from Breaking Your Site by Entering Long Strings of Data
```
/* YOUR CSS FILE */

.classForElementRenderingUserInput {
  word-break: break-all;
}
```

The CSS above will prevent a malicious user from breaking your page by shifting content too far to the left or right.
Additionally, we'll want to protect our site from being overrun with input that makes the height of a rendered element inhospitable. To this end, make this next statement your new design philosophy: EVERY input should have a MAX-LENGTH that one of your form validators should be checking for and ENFORCING!!! NO EXCEPTIONS!!! Even if the limit is high, it should be there!! NO EXCEPTIONS!!!
And finally, ALL constraints enforced on the frontend should match limits imposed by your database. Additionally, these validations should also be enforced on the backend. Malicious actors DON'T NEED YOUR FRONTEND TO INTERACT WITH YOUR SERVER!!! 
