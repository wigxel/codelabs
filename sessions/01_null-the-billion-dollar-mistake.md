# Null — The billion dollar mistake

# What is `null`?
This is a DataType in the Javascript programming language and is the major cause of program crashes in the Javascript ecosystem. It's also referred to as the Billion Dollar Mistake by the Creator — Tony Hoare

The base idea is that `null` made programmers write unsafe code which eventually caused systems to fail and greatly increases the cost of maintaining such systems.

## 01. Do you really understand `null`
Assuming we have a function that gets the user's name from a user object. 
```js
function getUsername(user) {
    if (typeof user !== 'object') return null;
    return user.name
}
```
Take a few seconds to look at the implementation of the function above. I'm sure you'll agree that there's nothing wrong with the code snippet above. 

### Let's dive deeper
Run the code snippet below in your browser console and view the output
```js
typeof null
```
We will come to realize that the typeof `null` is an `object` not `undefined` or `null`. 

Now, look at the code snippet from Case #1 and the flaw in the `getUsername()` will become more obvious.

## 02. `Null` and `undefined` are the same

They are a few misconceptions about `undefined` and `null` behaving the same. In my opinion, one can never be too careful with them. Run the code below before proceeding.
```js
let authUser = undefined;
let user = null

console.log(user === authUser); // type the output here
```
I often see Javascript programmers check for null-ish values using the code below. 

```js
function isAuthenticated(user) {
    if (!user) { // <-- See here
        return false;
    }
    return true;
}
```
The conditional above works well for null-ish values like `undefined`, `null` or an empty string. But not for an empty Object(`{}`), Array (`[]`) etc. You can verify from the code snippet below.

```js
const empty_string = "";
const null_value = null;
const undefined_value = undefined;
const empty_object = {};
const empty_array = [];
const empty_map = new Map();

console.log(!empty_string) // true
console.log(!null_value) // true
console.log(!undefined_value) // true
console.log(!empty_object) // what's the output
console.log(!empty_array) // what's the output
console.log(!empty_map) // what's the output
```

I personally prefer to use the conditional below to ease myself of the uncertainty.

```js
function isAuthenticated(user) {
    if ([undefined, null].includes(user)) return false;
    return true;
}
```
> Note: Extra caution is required when checking for null-ish values especially `undefined` and `null`. This little detail is the sole reason for many system/application crashes.

## 03. Working with `null` or `undefined` Object property values
Well for the most part when solving problems we use Javascript Objects. Let's see the problems while using `null` and `undefined` as property values.

```js
function getUsername(user) {
    if (typeof user === 'object') {
        return user.firstname.toUpperCase()
    }
    return null;
}

const user1 = { firstname: "John", lastname: "The Rock" }
getUserame(user1)
```
From the above snippet the goal is to get and format the `firstname` of the `user` parameter. "Problem solved nothing more to add" — Moje the programmer said. Little did Moje know that this is gonna cost his software to crash sooner or later.

#### So what's wrong with Moje's code?
Well, there's just one small issue. We can not guarantee that the `user.firstname` will always be present during the lifetime of the program.

Run the code code below and try fixing Moje's code to ensure it stops failing.
```js
function getUsername(user) {
    if (typeof user === 'object') {
        return user.firstname.toUpperCase()
    }
    return null;
}

// What if?
const user1 = { } // <-- An empty 
const user2 = null // <--- or possible null 

getUserame(user1)
getUsername(user2);
```

> Unless your object is hard-coded with the program. Ensure to expect this kind of error

#### Problem Set #1
Moje was asked to build a cart system where customer can add items to the cart and then fetch and display the customer's shipping details from an endpoint. So Moje starts out by making a simple store for all cart/checkout information.

```js
const cartStore = {
    items: [{ quantity: 1, item: { name: "Carrot" }}],
    deliveryAdress: null, // <-- 👀
};
```

Now Moje's `cartStore` looks very simple, since it just contains 2 properties namely `items` and `deliveryAdress`. Looking at the code we can see that `deliveryAdress` is set to `null` since the value isn't available yet.

```js
const cartStore = {
    items: [{ quantity: 1, item: { name: "Carrot" }}],
    deliveryAddress: null
}

// fetch the shipping address details from an endpoint
// eg. { street: "212 James Street" }
const shipping_details = await fetchShippingDetails();
cartStore.deliveryAddress = shipping_details;

// print the street address in all caps
cartStore.deliveryAddress.street.toUpperCase();
```
Moje is pretty happy that he finally solved the problem. But little does he know there are bugs in this solution. Take a few seconds to inspect the above code. See if you can discover the bugs.

### Problem Set #2
Run the code below. Discover and fix the bugs.  
> Hint: Be cautions of the `undefined`, `null` or `nullish` variables.

```js
function persistUser(user, options) {
   const opts = { 
     storage: "sessionStorage", 
     ...options
   };

   if (option.storage === 'sessionStorage') { //
     sessionStorage.setItem('userInfo', opt.serialize(user));
   }
   
   if (option.storage === 'localStorage') {
     localStorage.setItem('userInfo', opt.seralize(user));
   }
}

// Don't modify the code below
persistUser({ id: "42", firstName: "Kelvin" })
persistUser({ id: "42", firstName: "Kelvin" }, {}) 
```