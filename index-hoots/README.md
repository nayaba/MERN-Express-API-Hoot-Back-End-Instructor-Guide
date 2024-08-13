# Express API - Hoot Back-End - Index Hoots

## AAU, I should be able to read a list of all hoots.

**✔️ Create a `GET` route and router at `/hoots`**

**✔️ Define a controller function which finds all the Hoots**

**✔️ Test the route in Postman**

## Define the route

Our route will listen for `GET` requests on `'/hoots'`:
Inside `controllers/hoots.js`, add the following:

```jsx
// controllers/hoots.js

router.get('/', async (req, res) => {});
```

> ❗ A user needs to be logged in to view a list of hoots, so we should define our new route inside the **Protected Routes** section of `controllers/hoots.js`.

# ☑️ Check Trello (1/3)

## Code the controller function

Add the following to `controllers/hoots.js`:

```jsx
// controllers/hoots.js

router.get('/', async (req, res) => {
  try {
    const hoots = await Hoot.find({})
      .populate('author')
      .sort({ createdAt: 'desc' });
    res.status(200).json(hoots);
  } catch (error) {
    res.status(500).json(error);
  }
});
```
# ☑️ Check Trello (2/3)

## Test the route in Postman

Now that we have finished the route let's test it with Postman. We'll do this by sending a `GET` request to `http://localhost:3000/hoots`.

Within Postman, create a new `GET` request. We'll name this request **Index**.

Your Postman URL should look like this:

```
http://localhost:3000/hoots
```

If your request was successful, the response will include an array of `hoot` objects, with a populated `author` property inside each `hoot` object:

# ☑️ Check Trello (3/3)  ✅ - Move view a single hoot to DOING
