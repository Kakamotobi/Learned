# Uploading Files

## Table of Contents
- [Where to Upload Files](#where-to-upload-files)
- [HTTP POST Content Types](#http-post-content-types)
  - [`<form>` Elements](#form-elements)
- [Multer](#multer)

## Where to Upload Files
- Files should not be saved on the main server or the database.
- As the main server will frequently be destroyed and recreated as changes are made, all the files on the server will be lost.
- Databases are not meant to store the files itself. They are for storing the location(url) of the files.
- Instead, files should be saved in a separate server (Ex: Amazon).

## HTTP POST Content Types
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
### `<form>` Elements
- Uploading files are usually done through `<form>` elements.
- The content type in form elements can be indicated through the **`enctype`** attribute.
- Example
  ```html
  <form method="POST" enctype="multipart/form-data">
    <input type="file" accept="image/*" />
    <button type="submit">Upload</button>
  </form>
  ```

## Multer
- Multer is a Node.js middleware that allows feasible uploading of files.
- Multer takes the specified file from the input, then saves it in the specified destination, then passes on the file information to the controller, which can be accessed in `req.file`.
### Example
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

## Reference
[POST - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST)  
[Forms in HTML documents](https://www.w3.org/TR/html401/interact/forms.html)  
[RFC1341(MIME) : 7 The Multipart content type](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html)  
[multer - npm](https://www.npmjs.com/package/multer)  
