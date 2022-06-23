# Handling Files

## Table of Contents
- [Uploading Files](#uploading-files)
  - [Where to Upload Files](#where-to-upload-files)
  - [HTTP POST Content Types](#http-post-content-types)
    - [`<form>` Elements](#form-elements)
  - [Multer](#multer)
- [Files in Browser Memory](#files-in-browser-memory)
  - [`URL.createObjectURL()`](#urlcreateobjecturl)

## Uploading Files
### Where to Upload Files
- Files should not be saved on the main server or the database.
- As the main server will frequently be destroyed and recreated as changes are made, all the files on the server will be lost.
- Databases are not meant to store the files itself. They are for storing the location(url) of the files.
- Instead, files should be saved in a separate server (Ex: Amazon).

### HTTP POST Content Types
- When sending an HTTP POST request, the type of the body should be indicated.
  - When using the Fetch API, the `Content-Type` header is used to indicate the media type of the resource.
    - `"Content-Type": "application/json"`
      - Used for JSON text.
      - Ex: `{ "name": "Kakamotobi", "age": "2" }`.
    - `"Content-Type": "application/x-www-form-urlencoded"`
      - The default content type for form submission.
      - "&" and "=" are used to encode the key-value pairs.
      - Non-alphanumeric characters are percent encoded.
        - Hence, this content type is not suitable for binary data (use `multipart/form-data`).
      - Ex: `name=Kakamotobi&age=2`.
    - `"Content-Type": "multipart/form-data"`
      - A combination of one or more different data types in a single body.
      - Used for submitting forms that contain files, non-ASCII data, and binary data.
      - Each value is sent as a block of data ("body part").
      - A "boundary" is used as a delimiter to separate each part.
      - The keys are in the `Content-Disposition` header of each part.
      - Ex:
        ```
        Content-Type: multipart/form-data; boundary="iamboundary"
        
        --iamboundary
        Content-Disposition: form-data; name="name"
        
        Kakamotobi
        --iamboundary
        Content-Disposition: form-data; name="profile-picture"; filename="profilepicture.jpg"
        Content-Type: image/jpg
        
        content of the uploaded file profilepicture.jpg (binary digits of the image)
        --iamboundary--
        ```
    - `"Content-Type": "text/plain"`
      - Usually used only for debugging purposes.
      - White space represented as "+".
#### `<form>` Elements
- Uploading files are usually done through `<form>` elements.
- The content type in form elements can be indicated through the **`enctype`** attribute.
- Example
  ```html
  <form method="POST" enctype="multipart/form-data">
    <input type="file" accept="image/*" />
    <button type="submit">Upload</button>
  </form>
  ```

### Multer
- Multer is a Node.js middleware that allows feasible uploading of files.
- Multer takes the specified file from the input, then saves it in the specified destination, then passes on the file information to the controller, which can be accessed in `req.file`.
#### Example
```html
<form method="POST" enctype="multipart/form-data">
  <input name="avatar" type="file" accept="image/*" />
  <button type="submit">Upload</button>
</form>
```
```js
import express from "express";
import multer from "multer";
import { getEdit, postEdit } from "./controllers/userControllers.js";

const uploadFileMW = multer({ dest: "uploads/avatars/" });

const app = express();

app.use("/uploads", express.static("uploads"));
app.get("/user/edit", getEdit);
app.post("/user/edit", uploadFileMW.single("avatar"), postEdit);

export default app;
```

## Files in Browser Memory
### `URL.createObjectURL()`
- A static method that creates a unique object URL that represents the object provided in the parameter.
  - This object can be a File, Blob, or MediaSource object.
- The object URL is released when the document on which it was created unloads (stays in and unloads from browser memory).
  - It is a good idea to call `revokeObjectURL()` to manually release an object URL in order to achieve optimal performance and memory usage.
- It looks like a URL on the website domain but it is actually not.
  - Ex: `blob:http://localhost:4000/eca681e2-e1d8-43fc-a910-2622fa6f11bd`.
  - It is merely a temporary URL that provides access to the specific file that is currently in the memory of the browser.
#### Example
```html
<input type="file" id="file-input" accept="image/*" multiple />
```
```js
const imgInput = document.getElementById("img-input");

const handleFiles = () => {
  const firstFile = imgInput.value // selects the first file.
  const allFiles = imgInput.files; // a FileList object containing a list of file objects.

  const ul = document.createElement("ul");
  body.appendChild(ul);
  
  for (let file of this.allFiles) {
    const li = document.createElement("li");
    ul.appendChild(li);
    
    const img = document.createElement("img");
    img.src = URL.createObjectURL(file); // Generate object URL for file and use it to access the file.
    img.onload = function() {
      URL.revokeObjectURL(this.src);
    }
    li.appendChild(img);
    
    const imgCaption = document.createElement("span");
    imgCaption.innerText = `Title: ${file.name}, Size: ${file.size}`;
    li.appendChild(imgCaption);
  }
}

imgInput.addEventListener("change", handleFiles);
```

## Reference
[POST - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST)  
[Forms in HTML documents](https://www.w3.org/TR/html401/interact/forms.html)  
[RFC1341(MIME) : 7 The Multipart content type](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html)  
[multer - npm](https://www.npmjs.com/package/multer)  
[URL.createObjectURL() - Web APIS | MDN](https://developer.mozilla.org/en-US/docs/Web/API/URL/createObjectURL)  
