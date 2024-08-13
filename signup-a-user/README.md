# ![Express API - Hoot Back-End - Signup a User](./assets/hero.png)

- sign up a new user using Postman.

## Sign up

To get started, we'll use Postman and the existing Sign Up functionality provided by the auth template to add a new user to our database. 

Before diving into Postman, let's take note of the response (`res`) issued by our signup controller:

```js
// controllers/users.js

res.status(201).json({ user, token });
```

Notice the route returns a `token`. We'll add this `token` to a special tab in Postman, which will allow us to included it as a bearer token on future requests. This will be important, as beyond sign up and sign in, all other features in this application will be protected, requiring authenticated requests to access them.

## Test the route in Postman

1. In **Postman**, create a new collection called **Hoot**:

2. After creating the collection, locate the **Add a request** button:
    - We need to make a new Postman request called **Signup**. Start by updating the fields highlighted below:

    - Within the **Body** tab, select **raw**, and change the **Text** dropdown to **JSON**. 

    - Change the request type to a **POST** request, and provide the URL that matches the signup route:

    ```
    http://localhost:3000/users/signup
    ```

3. Be sure to click the **Save** button:

4. Add some test account information to the **Body** in **Postman**, as shown below:

```json
{
    "username": "test",
    "password": "test"
}
```

5. Copy the token, **do not include the quotes**.

# ☑️ Check Trello (4/4)

## Save the token

1. Click on the **Authorization** tab. 

    - Set the **Type** to **Bearer Token** 
    - Add your `token` to the **Token** input field. 

2. Afterwards, don't forget to click the **Save button**. 

> 💡 Depending on the width of your **Postman** window, you might need to click the dropdown menu in the screenshot below to access the **Save** option. Alternatively, the **Save** button will be represented by a floppy disk icon.

Congratulations - Now that we have a `user`, we can start to building out CRUD on our `hoots`.
