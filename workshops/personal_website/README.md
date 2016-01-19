# Personal Website

By the end of this workshop, you will learn to make your own personal website.
It will look something like this:

![](img/final_screenshot.png)

Open the [live demo][final-live-demo]. See the [final code][final-code].

[final-live-demo]: https://cdn.rawgit.com/anonymous/7a5a6cc614052c8c810f/raw/9419c5a5a4a871da091a49844ffbc46f434c01c8/index.html
[final-code]: http://jsbin.com/gist/7a5a6cc614052c8c810f?html,output

**On the way, you will:**

- Begin to learn the programming languages **HTML** & **CSS**
- Setup the coding tools, **GitHub** and **Cloud9**, which  you will use
  throughout your time in Hack Club

## Part 1: Setup

### 1) Sign Up for GitHub

**GitHub** is a website used by professional coders to collaborate on code.
Think Dropbox, but for code. We'll be using it to store and manage our code in
Hack Club.

1. Open https://github.com/join in a new tab and create a new account
  - Choose a username you will be proud to show future employers. (ex.
    `zachlatta` and `jonleung`)
  - Be careful to use an email that you have access to, you'll need to access it
    to verify your account
  - Make sure to remember your username and password, you'll need this at every
    subsequent club meeting
  - When prompted to choose a plan, make sure to choose the **"Free"** plan
2. Open your email inbox in a new tab and look for an email from GitHub. Open it
   and click the button to verify your email
3. Open https://github.com/hackclub/hackclub/ and click this button that looks
   like this on the top right: ![](img/github_star.png)

### 3) Signup for Cloud9

Just like you can use Google Drive to write and organize documents written in
English, we'll be using **Cloud9** to write, save, and organize our code.

> Note:
>
> The techy term for a place where you can write, and organize code is **"IDE"
or "Integrated Development Environment"**

1. In a new tab, open https://c9.io/
2. In the top right hand corner, click the button that looks like this: ![](img/c9_gh_icon.png)
3. Click the green **"Authorize application"** button
4. If there is a popup that asks for your email, go ahead and enter it
5. Click the grey box that says **"Create a new workspace"**
6. Set the **"Workspace name"**, to **"projects"**
7. Then without changing anything else on the screen, scroll to the bottom and
   click the green **"Create Workspace"** button.

You should now see a screen that looks like it's loading something like this:

![](img/c9_signup_ide_loaded.png)

> Note:
>
> If you don't see the above screen and after 10 seconds, it looks like
> the page is still trying to load, do the following:
>
> 1. Open https://c9.io
> 2. Click on the green **"Open"** button**
> 3. You should now see screen shown above. If not, ask your facilitator for
>    help

### 3) Celebrate!

> ![](img/celebrate_king_kid.gif)

Congratulations, you officially setup all of your coding tools for the semester!


Now we're ready to start the project!

## Part III: Create the Project Folder

For every project we make in Hack Club, we will be creating it in it's own
folder.

Let's create a folder for our personal website project:

1. On the left side of the screen, right click the **`projects`**
2. Click **"New Folder"**
3. Then name the folder **`personal_website`**

> ![](img/c9_create_personal_website_folder.gif)

## Part IV: HTML

Every website is written with **HTML** code. HTML code is written in **HTML
files**.

### 1) Create the HTML file for your personal website

1. Right click the **`personal_website`** folder you just created
2. Click **"New File"**
3. Then name the file **`index.html`**
4. Double click it to open it

> ![](img/c9_create_index_dot_html.gif)

> Notes:
>
> - Just like Microsoft Word Documents end in **`.doc`**, HTML files end in
**`.html**`
> - Your main html file has to be named **`index.html`**

### 2) Adding standard HTML template

Every HTML file essentially follows this template:

```html
<!DOCTYPE html>
<html>
    <head>
    </head>
    <body>
    </body>
</html>
```

Let's put this template inside of our HTML file:

1. Double click the **`index.html`** file you just created to open it
2. Copy and paste the above HTML code into your **`index.html`** file

> Note:
>
> You generally want to retype any code that you write because you will to
remember it better. However in this case, since there's so much there it's fine
to copy and paste it this time.

<html>


### 3) Adding to the Template

There's a lot going on in the below template. For now, let's just understand
that there are two sections in this template, the **head** and the **body**:

```markdown
<!DOCTYPE html>   
<html>          
    <head>
            ← "this section is called the **head** of the **html document**"
    </head>
    <body>
            ← "this section is called the **body** of the **html document**"
    </body>
</html>
```

- the **body** of the HTML document is where all the **content** belongs. This
  includes all of the words and images on the page.
- the **head** of the HTML document is where you set the settings for the page
  and include other code files you want on your website.



### 4) Opening the Website Preview

Let's see what our website looks like so far:

1. First, save the file by clicking **"File"** _(on the top right)_ → **"Save"**
   (or use the shortcut **CTRL + S / Command + S**)
2. Preview what the website looks like by clicking **"Preview"** → **"Live
   Preview File"**

> ![](img/c9_live_preview.gif)

As you can see, page is still blank. This is because we have not yet added any
**content** in the **`body`** section yet. Let's add some!

### 5) Adding My Name to the Body of the HTML

1. Write your name in the **body** of the html page (between the **`<body>`**
   and **`</body>`**). My name is `Drizzy Drake` so that's what I will write.

> ![](img/html_add_name.gif)

Notice that that the HTML code you write is immediately updated in the preview.
That's why it's called a **"Live Preview"**

### 6) Adding a Description

1. Underneath on a new line, write one short sentence about yourself. I love my
job as a call center representative so I'm going to write `MVP hotline
answerer`.

> ![](img/html_add_description.gif)

Notice that the blank space between my name and description isn't reflected on
the web page. Adding blank lines or spaces between your words
in HTML does will not change what your website looks like.

To add spacing, We need **HTML tags** for tags to format the way text look.

### 7) Formatting the Heading with the Heading Tag

**HTML tags** tell the computer how to interpret the text you write.

For example, if I want `Drizzy Drake` to be a heading of my webpage, I can
put it inside of a **heading tag**, like so:

```html
<h1>Drake Graham</h1>
```

1. Make your name look like a heading by surround your name with an `h1` heading
   tag.

    > Note that in Cloud9, as soon as you write  **`<h1>`** it knows you will
    eventually want a **`<h1>`** and so it just adds it for you. This is known
    as **autocomplete**.


> ![](img/html_format_name_as_h1_tag.gif)

There are two parts to the `h1` tag:

- **`<h1>`** is what's known as the the **opening**  tag
- **`</h1>`** is what's known as the the **closing** h1 tag.---The difference is
  the **`/`** before the closing tag

Because the browser knows to interpret anything inside an **`h1`** tag as a
heading, it makes the anything inside of the **`h1`** tag big and bold.

### 5) Formatting the Description with a Paragraph Tag

Similarly to how we told the browser to treat **`Drake Graham`** as a header, I
want to tell the browser to treat **`MVP hotline answerer.`** as a paragraph.

1. Put your description inside of a **paragraph tag** like so:

```html
<p>MVP hotline answerer</p>
```

> ![](img/html_format_description_as_p_tag.gif)

Just like **`h1`** tag, the **`p`** tag also has an opening and closing tag.

### 6) Adding an Image with the Image Tag

Now let's add an image to our website.

Instead of adding a picture of yourself right now, let's just use this picture
of me for now and you can change it later:

![](img/drake_100x100.gif)

Here's the URL for the image

```
https://surrogate.hackedu.us/i.imgur.com/S06cY9j.gif
```

To add the above image to the top of the page

1. Type the code for the **image tag** (written below) below the opening body
   tag:

  ```html
  <img src="https://surrogate.hackedu.us/i.imgur.com/S06cY9j.gif">
  ```

> ![](img/html_add_image_tag.gif)

> Notes:
>
> - **`img`** is the tag name
> - Note that **`img`** is a **self closing** tag, meaning that there is no
>   closing tag like **`</img>`**
> - **`src`** is an  **attribute** of the image tag and specifies the URL of the
    image

Hmm, our image is too big. CSS to the rescue!

## Part III: CSS

**HTML** is **content** of a page (things like, words, images, and structure).

**CSS** is the look and feel like (things like **color**, **spacing**, and
  **size**).

> Note:
>
> - **CSS** stands for **Cascading Style Sheet** and is sometimes referred to just
> as **style sheet**. Why is it called this? It is a **"sheet"**, the specifies
> all of your **"styles"**.

Therefore, if we want to change the **size** of the image → we use **CSS**!

### 1) Creating the CSS file

1. Right click the **`personal_website`** folder you had previously created
2. Click **"New File"**
3. Then name the file **`styles.css`**

> Note:
>
> - The **`.css` extension** tells the computer to interpret this as a `css`
>   file (just like `.doc` and `.html`)

> ![](img/css_add_css_file.gif)

### 2) Connecting the CSS file in the HTML file

Although we've created a CSS file, the HTML file will not automatically **link**
the CSS. We have to explicitly **link** the CSS file in the HTML.

To appropriately **link** the CSS file :

1. Type the code for the **link tag** (written below), into the **`head`**
   section of the code.

   ```html
   <link rel="stylesheet" href="styles.css">
   ```

> ![](img/css_add_link_tag.gif)

> Notes:
>
> - **`link`** is the tag name
> - Note that **`link`** tags always belong in the **`head`** of the html document
> - Note that **`link`** is **self closing tag** like **`img`**
> - **`href`** is an  **attribute** of the link tag and specifies the location
>   of the CSS file. It stands for **hypertext reference**
> - **`rel`** is also an **attribute** that tells the browser to interpret the
    linked file as a **style sheet**

### 3) Using CSS to Resize the Image

Now that we linked our CSS file, let's write some CSS code to make the image
smaller:

1. Open **`style.css`** by double-clicking it
2. Then add the below CSS code to make the image smaller by _typing_ the
   following code into the CSS file (don't copy and paste because you won't
   remember it as well)

    ```css
    img {
        width: 200px;
    }
    ```
3. To see what effect this had, save the file by clicking **"File"** →
   **"Save"** (or use the shortcut **CTRL + S / Command + S**)

Yay! Our image got smaller!

> ![](img/css_img_width.gif)

**Understanding The CSS More Deeply**

If we were to translate the code into English, this

```css
img {
    width: 200px;
}
```

 would roughly translate into:

- **Select** every **`img` tag** on this HTML page
  - and set the **width** to be **`200px`**

> Note:
>
> **`px`** stands for **pixels**. **Pixels** are the tiny dots dots that make up
> a computer screen. By setting the width to **200px**, we are saying we want
> every image on the page to span the length of 200 pixels (which is about 2
> inches on a screen). If you want to learn more about pixels, you can Google
> ["What is a pixel?"](https://www.google.com/#q=what+is+a+pixel)

### 4) Centering the Body

Then to center everything on the page

1. Type the below CSS code underneath our previous CSS code. _Remember, don't
   copy and paste._

   ```css
   body {
       text-align: center;
   }
   ```
3. Save the file by clicking **"File"** → **"Save"** (or use the shortcut
   **CTRL + S / Command + S**)

> ![](img/css_text_align_center.gif)

Now you can see that everything is centered on the page.

**Understanding More Deeply**

Why did this work? Notice that all of our code in the **body tag**.

And if we were to translate the code into English, this

```css
body {
    text-align: center;
}
```

 would roughly translate into:

- **Select** every **`body` tag** on this HTML page
  - and set the **alignment** of the **text** within the body tag to be
    **centered**

And because everything is in the **body** tag, everything is centered.

> Note
>
> - You can google ["css text align"]
  (https://www.google.com/?gws_rd=ssl#q=css+text+align) to see what other
  alignment options there are besides **`center`**

### 5) Changing the Font

Finally let's change the font of the page to **`Arial`**.

To do this:

Add another **css attribute** to the **body selector** so that your css code for
your **body** looks like this:

  ```css
  body {
      text-align: center;
      font-family: Arial;
  }
  ```

> ![](img/css_font_family_arial.gif)

> Note:
>
> - Not all computers have the same fonts on them. To figure out what fonts most
>   computers have on them, you can Google
>   ["web safe fonts"](https://www.google.com/#q=web+safe+fonts)

### 6) Celebrate!

![](img/celebrate_harry_potter.gif)

W0000t! You've successfully added the CSS to this web page!

Now to share your creation with the world.

## Part IV: Deployment

Right now we can only SEE our website on our own computer. Let's get a link that
we can share with anyone on the internet! ### 1) Hosting on Cloud9

In the **live preview** of the website:

1. Click the button that looks like this `TODO: Arrow Image`

   > ![](img/live_preview_button.png)

2. Then in the new window that pops up, copy the URL of the address bar
3. You can send this URL to anyone else in the world and they will be
   able to view your website!

**But there's a problem:**

This URL won't work indefinitely, it will stop working when you don't use your
Cloud9 for a couple of days. We want something that other people will be able to
open even if it's a year from now.

We can use **GitHub Pages** to **host** our website instead!

### 2) Hosting on GitHub Pages

GitHub allows us to **host** our website using a service called **GitHub
pages**. In simple terms, this means that you can put the files of your website
on GitHub and GitHub will give you a URL that you can share with the world.

#### a) Creating a GitHub repository on github.com

1. Open a new tab
2. Go to GitHub.com
3. Click the green **"+ New repository"** button
4. Under **"Repository name"** write your `username.github.io` except instead of
   writing `username`, write your actual GitHub username. So if your username
   is "alice1337", then you would write `alice1337.github.io`.
5. Make sure that you did the previous step correctly.
  - Is your **username** entered correctly
  - Does the repository name end in **`.io`**
6. After you are sure you entered in the repository name correctly, click the
   green "Create repository" button at the bottom.
7. Then copy the HTTPS link at the top. It should look like this:
   `https://github.com/username/username.github.io.git`

#### b) Setup in Cloud9

1. Open the terminal by pressing `alt + t` on the keyboard at the same time

  - `git init`
  - `git remote add origin https://github.com/username/username.github.io.git`
  - `git add --all`
  - `git commit -am "Initial commit"`
  - `git push -u origin master`
4. GitHub will now ask you for your username and password.
  - Go ahead and enter your username and then press the enter.
  - Then enter your password and press enter. _Note that the characters don't
    show up on the screen but rest assured, you are still typing._
5. Now try to view your website by going to `username.github.io/portfolio`

#### c) Celebrate!

![](img/celebrate_rush_hour.gif)

Yaaaaaass! Your website is now public on the internet and will stay!
Feel free to share this with your friends!

## Part VI: Sharing with the Community

Now that you have finished building your website

1. Follow these directions [here][slack] to Signup for our Slack in a new tab
2. Join your club's channel
3. Post the link to your website
  - In your club's channel
  - In the `#personal-website` channel

[slack]: ../../SLACK.md

## Part V: Hacktime

Alright, you've been following a decently rigid tutorial. Now is your chance to
to take this website we made together and make it your own.

Below are some websites that we found to give you some inspiration!

### Examples

**Websites Made By Other Hack Club Hackers**

- John Cena's Personal Website (created by
  [Brian Nguyen](http://nguyenbrian.github.io/john-cenas-personal-website/))
- http://jevinsidhu.com/
- https://mnefertiti.github.io/personalwhale/
- https://cydrobolt.com/

**Professional Websites**

- [Pushkar Modi](http://pushkarmodi.com)
- [Personal website of professional designer, Alice Lee](http://byalicelee.com)
- [Manuel Moreale](http://manuelmoreale.com)
- [Yaron Schoen](http://yaronschoen.com)

### Additional Resources

Here are some additional resources that you can use to learn the basics of HTML &
CSS.

| Resource                                                                          | Pros                                                                                     | Cons                                                                            |
|-----------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| [HTML&nbsp;Dog](http://www.htmldog.com/guides/html/beginner/)                     | Very beginner focused. If you're not sure which one of these to choose, choose this one. | Isn't too rich in content.                                                      |
| [Free&nbsp;Code&nbsp;Camp](http://www.freecodecamp.com/map)                       | Interactive and very methodical.                                                         | Not made for you to learn to make something that you want to show your friends. |
| [Team&nbsp;Tree&nbsp;House](https://teamtreehouse.com/library/html/introduction/) | Their videos are extremely comprehensive an through                                      | It takes a _very_ long time to get through and are very passive.                |
