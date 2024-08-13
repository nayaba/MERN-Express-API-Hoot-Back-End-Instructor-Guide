# Express API - Hoot Back-End - Show Hoot
## AAU, I should be able to view information about a single hoot post.

**✔️ Create a `GET` route and router at `/hoots/:hootId`**

**✔️ Define a controller function which finds the Hoot by its id**

**✔️ Test the route in Postman**


### Create a `GET` route and router at `/hoots/:hootId`

Inside `controllers/hoots.js`, we'll define a route to find a single `hoot` by `hootId`:

```jsx
// controllers/hoots.js

router.get('/:hootId', async (req, res) => {});
```

> ❗ A user needs to be logged in to view hoot details, so we should define our new route inside the **Protected Routes** section of `controllers/hoots.js`.

# ☑️ Check Trello (1/3)

### Code the controller function

Add the following to `controllers/hoots.js`:

```jsx
// controllers/hoots.js

router.get('/:hootId', async (req, res) => {
  try {
    const hoot = await Hoot.findById(req.params.hootId).populate('author');
    res.status(200).json(hoot);
  } catch (error) {
    res.status(500).json(error);
  }
});
```
# ☑️ Check Trello (2/3)

### Test the route in Postman

Now that we have finished the route let's test it with Postman. We'll do this by sending a `GET` request to `http://localhost:3000/hoots/:hootId`.

Create a new request in **Postman**. We will name this request **Show** and set its request type to `GET`. To test it out, we'll need to grab the value of a hoot `_id`.

Open up your **Index** request tab in **Postman**, and copy a hoot's `_id`. Add this value to your Postman URL for **Show**.

Afterwards, your Postman URL should look something like this:

```
http://localhost:3000/hoots/61b63d2e397b1f34f5861ebf
```

If the request was successful, your response will include a single `hoot` object.

☑️ Check Trello (3/3) ✅ - Move update a single hoot to DOING
