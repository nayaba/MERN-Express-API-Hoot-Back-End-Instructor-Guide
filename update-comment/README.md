# Express API - Hoot Back-End - Update Comment

## As the author of a comment, I should be able to delete that comment.

**✔️ Create a `PUT` route and router at `/hoots/:hootId/comments/:commentId`**

**✔️ Define a controller function which finds the comment by its id and updates it**

**✔️ Test the route in Postman and check the MongoDB database**

### Define the route

Add the following to `controllers/hoots.js`:

```js
// controllers/hoots.js

router.put('/:hootId/comments/:commentId', async (req, res) => {});
```

> 🧠 This route might be seem intimidating at first. It requires both a `hootId` and a `commentId`, so that we can locate both the parent, and the child document within it.

> ❗ A user needs to be logged in to update a comment, so we should define our new route inside the **Protected Routes** section of `controllers/hoots.js`.

# ☑️ Check Trello (1/3)

### Code the controller function

Let's breakdown what we'll accomplish inside our controller function.

First we call upon the `Hoot` model's `findById()` method.

We'll need to find the specific comment with the [MongooseDocumentArray.prototype.id()](https://mongoosejs.com/docs/api.html#mongoosedocumentarray_MongooseDocumentArray-id) method. 

With the retrieved `comment`, we update its `text` property with `req.body.text`, before saving the parent document (`hoot`).

Add the following to `controllers/hoots.js`:

```js
// controllers/hoots.js

router.put('/:hootId/comments/:commentId', async (req, res) => {
  try {
    const hoot = await Hoot.findById(req.params.hootId);
    const comment = hoot.comments.id(req.params.commentId);

    comment.text = req.body.text;

    await hoot.save();
    res.status(200).json({ message: 'Ok' });
  } catch (err) {
    res.status(500).json(err);
  }
});
```

# ☑️ Check Trello (2/3)

## Test the route in Postman

Create a new request called **Update Comment** and set the request type to `PUT`.

Your Postman URL should look something like this:

```
http://localhost:3000/hoots/63390dddff7c27bc4b86a1aa/comments/633915e08845c5a891cd4bf2
```

Add the following to the **Postman** body section.

```json
{
  "text": "my super cool update"
}
```

The response should be an object containing a `message: "Ok"` property:

# ☑️ Check Trello (3/3)  ✅ - Move view a single hoot to DOING
