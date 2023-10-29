# For A Good User Experience, Call (555-CSS-GOOD)


## Consideration #1: Shifting Page Content
A common issue seen in the apps made by new web devs is the LACK of consideration for page content moving around when being acted upon by the user.
This is most commonly observed in a couple of areas:
1. Rendering validation error messages on a form.
2. Adding or removing borders from elements that have been selected or unselected.

Generally speaking, there are 2 methods for rendering validation error messages on a form. Either 1) Render a parent element at the top of the form, inside of which, you will map over an array of pertinent error messages OR (my personal preference) 2) rendering an error message immediately above or below the concerned input field.
In both cases, when error messages are rendered on the form, the form grows either in size (height: fit-content) and/or elements within the form are moved from their current positions to make room for the error messages. AND in both cases, this issue can be remedied by acoounting for that space BEFORE the messages are rendered. In case #1, where we render messages at the top of the form, we must first figure out how much space the messages will take when ALL of the error messages are rendered at once. With this value in hand, we can simply set the CSS property 'min-height' on that parent element, so that it is always taking up that much space on the form regardless of error messages being rendered or not. The solution to case #2 is essentially the same. Only in this case, we are really just getting the height of a SINGLE error message. Depending on your intentions for the site layout and whatever particular use case you might have, this value can be a fixed number of pixels or a % of the containing element. To accomodate all of the various screen sizes available (mobile, oh my!!!), it is recommended to have a MINIMUM and MAXIMUM size for most elements. The minimum and maximum values should be related to a minimum and maximum content width. Widths that fall between these two bounds, should be adjusted DYNAMICALLY!! HINT: Either by adjusting your browser manually or by using Chrome's builtin screen sizes for popular mobile devices, you can figure out what minmum and maximum sizes fit your application. Also note that aside from adjusting the size of elements, you'll also want to adjust the size of the text on your page! Let's explore some generic solutions for these issues! 

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

## Consideration #4: Disabling the Submit Button
As a general rule, disabling the Submit button on a form UNTIL the user has satisfied input criteria is a TERRIBLE User Experience!!
If this approach REALLY speaks to you and you want to incorporate it in a way that is acceptable to the User Experience, you will need to follow the guidelines set forth below.

1. Either render instructions at the top of the form OR near each input that explains WHAT the user needs to do in order to satisfy the input criteria.
2. As an alternative to the above, you'll need render validation errors AS the user is typing!! The error messages should render as soon as the user begins typing in a particular input and should disappear when an input validator has passed!

A user should NEVER have to figure out how our app works! It is our job to GUIDE them through the apps functionality in as clear and simple of a way as possible! Disabling a Submit button and then leaving it to the user to figure out, is a sadistic act!! 

## Consideration #5: Labelling Inputs

Rule of Thumb: ALL form inputs should be labelled AT ALL TIMES!!! Using placeholder text which disappears the second a user types into it is considered bad practice. With this mind, you really only have two options. 1) Label the inputs and forget about it! 2) Imitate placeholder text by using CSS positioning. With this method, the user will see an input that APPEARS to be labelled with placeholder text, but once they click into the input (focus it), the "placeholder" label will move somewhere else so that the user can type but see the label at the same time. The label could shift into the border (shown below), could move to the left or right of the input, or up and out and above or below the input.

```
{/* YOUR FORM COMPONENT */}

<div className="input-containers">
  <input
    type="text"
    value={title}
    onChange={(e) => titleWrap(e)}
    className="post-inputs"
    required
  />
  <div className="floating-placeholders" style={ title ? { top: "-10.5px" } : null }><label>Title</label></div>
  <div className="post-errors-container">
    <div className="post-errors">{titleError}</div>
  </div>
</div>

{/* Note that the inline styling on the div with className "floating-placeholders" is what KEEPS the label up and into the border of the input when we have text in the field and we click out of the input (onBlur)
    Also note that the value "-10.5px" works for MY implementation! Your values will very likely be different depending on what you want your interface to look like. Adjust to your liking!
    Let's look at some generic CSS values below!
*/}

/* YOUR CSS FILE */

.input-containers {
  position: relative;
}

.floating-placeholders {
  position: absolute;
  pointer-events: none; /* We don't want the label to be recognized on hover or click */
  top: <Whatever pixel value puts the label in the center of your input field along the vertical axis>
  left: <Whatever pixel value places the label in the desired position on the horizontal axis>
}

.post-inputs:focus+.floating-placeholders {
  top: <same pixel value in the conditional render in the "top" property in the inline style on the "floating-placeholders" div. In my case, -10.5px>
}

```
