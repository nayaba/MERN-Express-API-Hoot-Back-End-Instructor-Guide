# Express API - Hoot Back-End - Build the Models

## AAU, I should be able to create a hoot post.
**✔️ Create the model based on the schema defined in the project ERD**

**⭕ Create a test route and router at `/hoots`**

**⭕ Test the route in Postman and check the database for a new Hoot**

### Create the model file `models/hoot.js`

Next we'll add the `models/hoot.js` file.

Run the following command in your terminal:

```bash
touch models/hoot.js
```

> 💡 We use a **singular** naming convention for model files, as a single file will always export just one model.

✨ Open the ERD to define the Hoot schema ✨

## Create the `hootSchema`

Before we're able to define our schema and model, we must first import the `mongoose` library into `models/hoot.js`:

```js
// models/hoot.js

const mongoose = require('mongoose');
```

Next we'll define the schema, which will act as a blueprint for the shape of `hoot` data in our database.

Update `models/hoot.js` with the following:

```js
// models/hoot.js

const hootSchema = new mongoose.Schema(
  {
    title: {
      type: String,
      required: true,
    },
    text: {
      type: String,
      required: true,
    },
    category: {
      type: String,
      required: true,
      enum: ['News', 'Sports', 'Games', 'Movies', 'Music', 'Television'],
    },
    author: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
  },
  { timestamps: true }
);
```

> 💡 Notice this inclusion of `{ timestamps: true }`. This will give our `hoot` documents `createdAt` and `updatedAt` properties. We can use the `createdAt` property when we want to display the date a hoot post was made.

### Introducing Enum

- **What is Enum?**
  - `Enum`, short for "enumeration," is a special data type that restricts a variable to have one of only a few predefined values.
  - Think of it as a way to ensure that a property can only be set to a limited set of possible values, which helps maintain data integrity and consistency.

- **Why Use Enum for Category?**
  - By using an enum for the `category` property, we can limit the values to a specific list, such as `['News', 'Sports', 'Games', 'Movies', 'Music', 'Television']`.
  - This restriction ensures that users or developers cannot mistakenly assign a category outside of these predefined options.


### Real-World Analogy

Imagine a dropdown menu for selecting a category in a blog post creation form. By using an enum, you ensure that the dropdown only offers valid categories, preventing users from entering arbitrary text and maintaining consistency across all posts.  (You might think what's the point of doing this if you make a drop down menu on your front end, but ee're keeping in mind hackers like Denis who might manipulate our front end)


## Register the model

```js
// models/hoot.js

const Hoot = mongoose.model('Hoot', hootSchema);
```

## Export the model

```js
// models/hoot.js

module.exports = Hoot;
```

# ☑️ Check Trello (1/3) - Create the model based on the schema defined in the project ERD
