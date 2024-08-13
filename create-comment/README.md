# Express API - Hoot Back-End - Create Comment

## AAU, I should be able to create a comment and add it to a specific hoot.

**✔️ Create the model based on the schema defined in the project ERD & embed it in the Hoot model**

**✔️ Create a `POST` route and router at `/hoots/:hootId/comments`**

**✔️ Define a controller function which creates a Comment**

**✔️ Test the route in Postman and check the MongoDB database**

## Create the `commentSchema`

Add the following to `models/hoot.js`:

```js
// models/hoot.js

const commentSchema = new mongoose.Schema(
  {
    text: {
      type: String,
      required: true
    },
    author: { type: mongoose.Schema.Types.ObjectId, ref: 'User' }
  },
  { timestamps: true }
);
```

Next we'll need to update the `hootSchema` with a `comments` property:

```js
// models/hoot.js

    author: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
    comments: [commentSchema]
  },
  { timestamps: true }
);
```

# ☑️ Check Trello (1/4)


### Define the route

We'll need to include the `hootId` as a parameter here, so that the new comment can be added to the appropriate **parent document**.

```js
// controllers/hoots.js

router.post('/:hootId/comments', async (req, res) => {});
```

> ❗ A user needs to be logged in to create a comment, so we should define our new route inside the **Protected Routes** section of `controllers/hoots.js`.

# ☑️ Check Trello (2/4)

### Code the controller function

We'll first append `req.user._id` to `req.body.author`. 

Next we'll call upon the parent Hoot with the `findById()` method.

Because `comments` are embedded, we cannot call upon the `create()` method. Instead, we'll use the `Array.prototype.push()` method on `req.body`, and add the comment to the `comments` array.

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

# ☑️ Check Trello (3/4)

### Test the route in Postman

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

# ☑️ Check Trello (4/4)  ✅ - Move view a single comment to DOING
