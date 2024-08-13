# Express API - Hoot Back-End - Update Hoot

## As the author of a hoot, I should be able to update that hoot.

**✔️ Create a `PUT` route and router at `/hoots/:hootId`**

**✔️ Define a controller function which finds a Hoot by its id and updates it**

**✔️ Test the route in Postman**

### Define the route

Add the following to `controllers/hoots.js`:

```js
// controllers/hoots.js

router.put('/:hootId', async (req, res) => {});
```

> ❗ A user needs to be logged in to update a hoot, so we should define our new route inside the **Protected Routes** section of `controllers/hoots.js`.

# ☑️ Check Trello (1/3)

### Code the controller function

If the `user` has permission to update the resource, we'll call upon our `Hoot` model's `findByIdAndUpdate()` method.

Add the following to `controllers/hoots.js`:

```js
// controllers/hoots.js

router.put('/:hootId', async (req, res) => {
  try {
    // Find the hoot:
    const hoot = await Hoot.findById(req.params.hootId);

    // Check permissions:
    if (!hoot.author.equals(req.user._id)) {
      return res.status(403).send("You're not allowed to do that!");
    }

    // Update hoot:
    const updatedHoot = await Hoot.findByIdAndUpdate(
      req.params.hootId,
      req.body,
      { new: true }
    );

    // Append req.user to the author property:
    updatedHoot._doc.author = req.user;

    // Issue JSON response:
    res.status(200).json(updatedHoot);
  } catch (error) {
    res.status(500).json(error);
  }
});
```

> 💡 As an extra layer of protection, we’ll use conditional rendering in our React app to limit access to this functionality so that only the author of a hoot can view the UI elements that allow editing.

# ☑️ Check Trello (2/3)

### Test the route in Postman

Now that we have finished the route, let's test it with Postman. We'll do this by sending a `PUT` request to `http://localhost:3000/hoots/:hootId`.

Create a new request in Postman. We will name this request **Update** and set its request type to `PUT`. To test it out, we'll need to grab a hoot `_id` again. Feel free to use the same Postman URL we used for **Show**.

```plaintext
http://localhost:3000/hoots/65f88f758b9a40bd02dacdbc
```

Add the following to the body section in **Postman**.

```json
{
  "text": "My updated hoot text!"
}
```


After sending the request, you should see the updated `hoot` object issued in the response

# ☑️ Check Trello (3/3)  ✅ - Move delete to DOING
