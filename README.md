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

Rule of Thumb: ALL form inputs should be labelled AT ALL TIMES!!! Using placeholder text which disappears the second a user types into it is considered bad practice. With this in mind, you really only have two options. 1) Label the inputs and forget about it! 2) Imitate placeholder text by using CSS positioning. With this method, the user will see an input that APPEARS to be labelled with placeholder text, but once they click into the input (focus it), the "placeholder" label will move somewhere else so that the user can type but see the label at the same time. The label could shift into the border (shown below), could move to the left or right of the input, or up and out and above or below the input.

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

## Consideration #6: AWS for Image Uploads (Including Pre-Population of Edit form with Image)

A common issue that trips students up when implementing AWS for image uploads is the EDIT FORM!! Pre-populating the form with the Image and especially the original file name (if applicable) poses some interesting challenges. Below we shall offer some generic solutions for this issue. Examples will be given for using a file input that reflects the selected filename on the screen and one that simply renders a thumbnail of the selected file. In addition to these concerns, we'll address validating this data in our endpoint using wtforms. The example will show a custom validator function that will handle cases where the user has NOT chosen a file to upload and the case in which they have. If file uploads will be mandatory in your application, your form may look different and will probably benefit from simply using the validators builtin to WTForms (Refer to Brad's AWS walkthrough, if so).
```
Input WITH filename (NOTE: To pre-populate the Edit Form with the original filename (we use random keys in AWS), we will need to save the original filename as a column in our database for this row!)

{/* OUR FORM COMPONENT employing a file input that is styled as a button with a small thumbnail element that will render the selected file above it.
Additionally we are positioning an element ABOVE the default file label that typically displays the filename.
As we cannot programmatically set this value (the browser simply doesn't allow it), we must imitate this behavior with our own element.
In this case, the <div> with the className of "file-inputs-filename". */}

<div className="file-inputs-container">
  <input type="file" accept="image/png, image/jpeg, image/jpg" id="post-image-input" onChange={fileWrap}></input>
  <div className="file-inputs-filename" style={{ color: filename === maxFileError ? "red" : "#B7BBBF" }}>{filename}</div>
  <div className="file-inputs-optional">{optional}</div>
  <div style={{ position: "absolute", top: "-34px", left: "39px"}}><img src={imageURL} className="thumbnails"></img></div>
  <label htmlFor="post-image-input" className="file-input-labels">Choose File</label>
</div>

/* OUR CSS FILE */

.file-inputs-container {
  position: relative; /* This allows the elements within it to be positioned without taking this element out of the document flow. As it's parent is NOT positioned, it only effects the children. */
  margin-bottom: 32px;
}

#post-image-input {
  visibility: hidden;
  /* Here we make the default file input button invisible on the page.
     As the label for this input is connected to it using the htmlFor property, we can style the label as a button instead, and have it behave the same as the <input type="file" />
     This extra step is necessary as the default <input type="file" /> CANNOT be styled with CSS!!
  */
}

/*
  Important note here is that the element is positioned "absolute".
  You will need to adjust the top and left properties to get the label to appear where you want it on your page.
 */

.file-inputs-filename {
  position: absolute;'
  top: 40px;
  left: 144px;
  z-index: 2;
  color: var(--minimal-border);
  background-color: var(--minimal-background);
  font-size: 14px;
  min-width: 90px;
  padding-bottom: 2px;
}

/*
  This label was added to clarify for the user that uploading an image is OPTIONAL.
  Feel free to implement this anyway that makes sense for you. Including NOT doing it!!
*/

.file-inputs-optional {
  position: absolute;
  font-size: 14px;
  color: red;
  top: 41px;
  left: 240px;
}

/*
  Here we set fixed dimensions for the thumbnail. Size it to your liking!
  Positioning for this element has been done inline, in the hopes of making this class more re-usable.
  Feel free to position it in the CSS file if it suits your aims.
*/

.thumbnails {
  width: 50px;
  height: 50px;
  outline: none;
}

/*
  The 2 directives below style our label like a button and give it a nice on hover effect.
*/

.file-input-labels {
  display: block;
  border: 2px solid var(--minimal-border);
  border-radius: 4px;
  color: var(--minimal-border);
  background-color: var(--minimal-background);
  width: 124px;
  height: 40px;
  font-size: 16px;
  font-weight: 600;
  padding-top: 7px;
  transition: all 0.3s ease-in-out;
}

.file-input-labels:hover {
  color: white;
  border-color: white;
  cursor: pointer;
}
```
```
Input WITHOUT filename (This implementation is noticably simpler than the one above, FYI.)
{/* OUR FORM COMPONENT employing a file input that is styled as a button with a small thumbnail element that will render the selected file inside of it. */}

<div className="file-inputs-container">
  <input type="file" accept="image/png, image/jpeg, image/jpg" id="post-image-input2" onChange={fileWrap2}></input>
  <label htmlFor="post-image-input2" className="file-input-labels-noname"><img src={imageURL2} className="thumbnails-noname"></img></label>
</div>

/* OUR CSS FILE */

/* Same as above */
.file-inputs-container {
  position: relative; /* This allows the elements within it to be positioned without taking this element out of the document flow. As it's parent is NOT positioned, it only effects the children. */
  margin-bottom: 32px;
}

/*
  As the htmlFor property requires a unique id, here we simply add a 2 to the end of the original id name. Unless you are allowing multiple images, this is not actually necessary.
  One file input could be named anything that you desire. Just note that if you have multiple, they all need different names!
*/

#post-image-input2 {
  visibility: hidden;
}

/*
  As before, we simply style the label as a button
  In this configuration, the button itself will also provide the thumbnail.
*/

.file-input-labels-noname {
  position: absolute;
  top: -48px;
  right: 0px;
  display: block;
  border-radius: 6px;
  color: var(--minimal-border);
  background-color: var(--minimal-background);
  width: 120px;
  height: 120px;
  transition: all 0.3s ease-in-out;
}

.file-input-labels-noname:hover {
  cursor: pointer;
}

/*
  In this configuration, I've chosen to put the border on the image tag.
*/

.thumbnails-noname {
  width: 120px;
  height: 120px;
  border: 2px solid var(--minimal-border);
  border-radius: 6px;
}

```

ADDITIONAL NOTE: BOTH of these methods employ a default image that is set as the src property of the thumbnail <img> tag BEFORE a file is selected. In both cases I am using a screenshot that I took of the site so that I could use a file that was the same color as the site background. In the case of the button WITHOUT the filename labels, I added the PLUS icon to the page before taking the screen shot, so that it clarifies for the user that they can click on the input. Do what feels right for you.

And finally, let's look at a custom validator for WTForms on our backend. Note that in MOST scenarios, your feature will probably REQUIRE an image and therefore it is more practical to use the builtin validators that ship with WTForms. However, for the Edit, you may want to employ something similar to this as it allows for a value of None in the FileField which is typical of an Edit form!!

```
allowed_extensions = { 'jpg': True, 'jpeg': True, 'gif': True, 'png': True }

def find_file_extension(filename):
    ext = ""
    for c in reversed(filename):
        if c != '.':
            ext = c + ext
        else:
            break

    return ext

def image(form, field):
    image = field.data

    if image == None:
        return True # Because a NEW file upload is not necessary on update.

    image.seek(0, os.SEEK_END) # move the cursor to the final byte of the file
    file_length = image.tell() # Get the size of the file in bytes!
    image.seek(0) # reset the cursor to the beginning of the file. Otherwise we will upload NO bytes to the s3 bucket!!

    if file_length > 5000000:
        raise ValidationError('File exceeds the maximum size of 5Mb')

    file_extension = find_file_extension(image.filename)
    if allowed_extensions[file_extension]:
        return True
    else:
        raise ValidationError('Allowed file types are .jpg, .jpeg, .png, .gif')

class PostForm(FlaskForm):
  title = StringField('title', validators=[DataRequired(), title])
  content = StringField('content', validators=[DataRequired(), content])
  filename = StringField('filename')
  image = FileField('image', validators=[image])
```
