# Express API - Hoot Back-End - Create Comment

## AAU, I should be able to create a comment and add it to a specific hoot.

**✔️ Create a `GET` route and router at `/hoots`**

**✔️ Define a controller function which finds all the Hoots**

**✔️ Test the route in Postman**
## Define the route

Our route will listen for `POST` requests on `/hoots/:hootId/comments`:

```
POST /hoots/:hootId/comments
```

We'll need to include the `hootId` as a parameter here, so that the new comment can be added to the appropriate **parent document**.

Add the following to `controllers/hoots.js`:

```js
// controllers/hoots.js

router.post('/:hootId/comments', async (req, res) => {});
```

> ❗ A user needs to be logged in to create a comment, so we should define our new route inside the **Protected Routes** section of `controllers/hoots.js`.

## Code the controller function

Let's breakdown what we'll accomplish inside our controller function.

As we did when creating hoots, we'll first append `req.user._id` to `req.body.author`. This updates the form data that will be used to create the resource, and ensures that the logged in user is marked as the `author` of a `comment`.

Next we'll call upon the `Hoot` model's `findById()` method. The retrieved `hoot` is the parent document we wish to add a comment to.

Because `comments` are embedded inside `hoot`'s, the `commentSchema` has not been compiled into a model. As a result, we cannot call upon the `create()` method to produce a new comment. Instead, we'll use the `Array.prototype.push()` method, provide it with `req.body`, and add the new comment data to the `comments` array inside the `hoot` document.

To save the comment to our database, we call upon the `save()` method of the `hoot` document instance.

After saving the `hoot` document, we locate the `newComment` using its position at the end of the `hoot.comments` array, append the `author` property with a `user` object, and issue the `newComment` as a JSON response.

Add the following to `controllers/hoots.js`:

```js
// controllers/hoots.js

router.post('/:hootId/comments', async (req, res) => {
  try {
    req.body.author = req.user._id;
    const hoot = await Hoot.findById(req.params.hootId);
    hoot.comments.push(req.body);
    await hoot.save();

    // Find the newly created comment:
    const newComment = hoot.comments[hoot.comments.length - 1];

    newComment._doc.author = req.user;

    // Respond with the newComment:
    res.status(201).json(newComment);
  } catch (error) {
    res.status(500).json(error);
  }
});
```

## Test the route in Postman

Create a new request called **Create Comment** and set the request type to `POST`.

> ❗ This request requires authentication, so ensure the **Authorization** tab is configured correctly.

Your Postman URL should look something like this:

```
http://localhost:3000/hoots/61b646f65f455c912b4f2f8d/comments
```

Add the following to the body in **Postman.** Within the **Body** tab, select **raw**, and change the **Text** dropdown to **JSON**.

```json
{
  "text": "my super cool comment"
}
```

After completing the steps above, your request in **Postman** should look something like this:

![Create comment request](./assets/comment-req.png)

A successful response will look like the following:

![Create comment response](./assets/comment-res.png)
