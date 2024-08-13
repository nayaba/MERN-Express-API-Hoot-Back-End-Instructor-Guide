# Express API - Hoot Back-End - Delete Hoot

## As the author of a hoot, I should be able to delete that hoot.

**✔️ Create a `DELETE` route and router at `/hoots/:hootId`**

**✔️ Define a controller function which finds a Hoot by its id and deletes it**

**✔️ Test the route in Postman and check the MongoDB database**

### Define the route

Add the following to `controllers/hoots.js`:

```jsx
router.delete('/:hootId', async (req, res) => {});
```

> ❗ A user needs to be logged in to delete a hoot, so we should define our new route inside the **Protected Routes** section of `controllers/hoots.js`.

# ☑️ Check Trello (1/3)

### Code the controller function

If the `user` has permission to delete the resource, we call upon our `Hoot` model's `findByIdAndDelete()` method.

```js
// controllers/hoots.js

router.delete('/:hootId', async (req, res) => {
  try {
    const hoot = await Hoot.findById(req.params.hootId);

    if (!hoot.author.equals(req.user._id)) {
      return res.status(403).send("You're not allowed to do that!");
    }

    const deletedHoot = await Hoot.findByIdAndDelete(req.params.hootId);
    res.status(200).json(deletedHoot);
  } catch (error) {
    res.status(500).json(error);
  }
});
```

> 💡 As an extra layer of protection, we’ll use conditional rendering in our React app to limit access to this functionality so that only the author of a hoot can view the UI element for deleting.

# ☑️ Check Trello (2/3)

### Test the route in Postman

Now that we have finished the route let's test it with Postman. We'll do this by sending a `DELETE` request to `http://localhost:3000/hoots/:hootId`.

Create a new request in Postman. Let's name this request **Delete** and set its request type to `DELETE`. To test it out, we'll need to grab a hoot `_id` again. Feel free to use the same Postman URL we used for **Show**.

Your Postman URL should look something like the following:

```
http://localhost:3000/hoots/61b63d2e397b1f34f5861ebf
```

After sending the request, you should see the deleted hoot object as a response.

Congratulations! 🎉 We are finished building out **CRUD** functionality for `hoots`!

# ☑️ Check Trello (3/3)  ✅ - Move add a comment to DOING
