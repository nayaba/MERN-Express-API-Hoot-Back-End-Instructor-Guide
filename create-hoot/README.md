# Express API - Hoot Back-End - Create Hoot

## AAU, I should be able to create a hoot post.
**✔️ ~~Create the model based on the schema defined in the project ERD~~**

**✔️ Create a test route and router at `/hoots`**

**✔️ Test the route in Postman and check the database for a new Hoot**

### Create a test route and router at `/hoots`

```jsx
// server.js

app.post('/hoots', (req, res) => {res.send('The /hoots route is working'});
```

### Test the route in Postman

In **Postman**, make a new `POST` request called **Create**.
 - Change the request type to a **POST** request, and provide the URL that matches the signup route:

     ```
     http://localhost:3000/hoots
     ```

Be sure to click the **Save** button

### Create a `hootsRouter`

```bash
touch controllers/hoots.js
```

Add the following boilerplate to `controllers/hoots.js`.

```jsx
// controllers/hoots.js

const express = require('express');
const verifyToken = require('../middleware/verify-token.js');
const router = express.Router();

// ========== Public Routes ===========

// ========= Protected Routes =========

router.use(verifyToken);

module.exports = router;
```

Cut our route from server.js and make some adjustments
```js
// app ---> router and '/hoots' ---> '/'
// ========= Protected Routes =========
router.use(verifyToken)
router.post('/', (req, res) => {
    res.send('The /hoots route is working')
})
```

> 🏆 Any route that requires auth should go below `router.use(verifyToken)`. These are our **Protected Routes**. A user needs to be logged in to create a hoot, so we should define our new route inside the **Protected Routes** section of `controllers/hoots.js`.

In `server.js`, let's import the `hootsRouter` and add it to our `'/hoots'` route.

Add the following to `server.js`:

```jsx
// server.js

const hootsRouter = require('./controllers/hoots.js');
```

And mount the router:

```jsx
// server.js

app.use('/hoots', hootsRouter);
```

### Test the route in Postman

# ☑️ Check Trello (2/3)

### Code the controller function

Add the following to `controllers/hoots.js`:

```jsx
// controllers/hoots.js

router.post('/', async (req, res) => {
  try {
    // logged in user is marked as the `author` of a `hoot`
    req.body.author = req.user._id;
    const hoot = await Hoot.create(req.body);
    res.status(201).json(hoot);
  } catch (error) {
    console.log(error);
    res.status(500).json(error);
  }
});
```

Now that we're using the Hoot Model, be sure to import it at the top:
```js
const Hoot = require('../models/hoot.js');
```

### Test the route in Postman

Now that we have finished the route let's test it with Postman. We'll do this by sending a `POST` request to `http://localhost:3000/hoots`.

Within the **Body** tab, select **raw**, and change the **Text** dropdown to **JSON**. Next, add the following test data to the body:

```json
{
  "title": "Big News",
  "category": "News",
  "text": "hoot Text"
}
```

### Check MongoDB for our newly created Hoot


# ☑️ Check Trello (3/3) ✅ - Move read a list to DOING
