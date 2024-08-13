# Express API - Hoot Back-End - Delete Comment

## As the author of a comment, I should be able to delete my own comment.

**✔️ Create a `DELETE` route and router at `/hoots/:hootId/comments/:commentId`**

**✔️ Define a controller function which finds the comment by its id and deletes it**

**✔️ Test the route in Postman and check the MongoDB database**

### Define the route

Add the following to `controllers/hoots.js`:

```js
// controllers/hoots.js

router.delete('/:hootId/comments/:commentId', async (req, res) => {});
```

> 🧠 This route might be seem intimidating at first. It requires both a `hootId` a `commentId`, so that we can locate both the parent, and then the child document within it.

> ❗ A user needs to be logged in to update a comment, so we should define our new route inside the **Protected Routes** section of `controllers/hoots.js`.

# ☑️ Check Trello (1/3)

### Code the controller function

It's very similar to the update controller.

Add the following to `controllers/hoots.js`:

```js
// controllers/hoots.js

router.delete('/:hootId/comments/:commentId', async (req, res) => {
  try {
    const hoot = await Hoot.findById(req.params.hootId);
    hoot.comments.remove({ _id: req.params.commentId });
    await hoot.save();
    res.status(200).json({ message: 'Ok' });
  } catch (err) {
    res.status(500).json(err);
  }
});
```
# ☑️ Check Trello (2/3)

### Test the route in Postman

Create a new request in **Postman**. Let's name this request **Delete Comment** and set its request type to `DELETE`. Your **Postman** URL should look something like this.

```
http://localhost:/hoots/63390dddff7c27bc4b86a1aa/comments/633915e08845c5a891cd4bf2
```


The response should be an object containing a `message: "Ok"` property

# ☑️ Check Trello (3/3)  ✅ - WE DONE!!!!
