--- 
draft : false
date : 2024-02-04T14:37:18+01:00
title : "Start Appwrite with NextJS"
description : "Setup Your First NextJS/AppWrite Project"
tags : ["Appwrite", "Nextjs"]
showTableOfContents : true
type: "post"
---

Learn how to setup your first Next.js project powered by Appwrite.

## Create project
Head to the Appwrite Console.
![Appwrite Console](/images/20240203project-id.png)

If this is your first time using Appwrite, create an account and create your first project.

Then, under **Add a platform**, add a **Web app.** The **Hostname** should be `localhost`.

![Add Platform](/images/20240203add-platform.png)

You can skip optional steps.

## Create Next.js project

Create a Next.js project and create a JavaScript Next.js project.

```bash
npx create-next-app@latest && cd my-app
```

## Install Appwrite SDK

Install the JavaScript Appwrite SDK.

```bash
npm install appwrite
```

## Define Appwrite service

Find your project's ID in the Settings page.

![Project ID](/images/20240203project-id.png)

Create a new file `app/appwrite.js` and add the following code to it, replace `<YOUR_PROJECT_ID>` with your project ID.

```javascript
import { Client, Account } from 'appwrite';

export const client = new Client();

client
    .setEndpoint('https://cloud.appwrite.io/v1')
    .setProject('<YOUR_PROJECT_ID>'); // Replace with your project ID

export const account = new Account(client);
export { ID } from 'appwrite';
```

## Create a login page
Create or update `app/page.js` file and add the following code to it.
```jsx
"use client";
import { useState } from "react";
import { account, ID } from "./appwrite";

const LoginPage = () => {
  const [loggedInUser, setLoggedInUser] = useState(null);
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [name, setName] = useState("");

  const login = async (email, password) => {
    const session = await account.createEmailSession(email, password);
    setLoggedInUser(await account.get());
  };

  const register = async () => {
    await account.create(ID.unique(), email, password, name);
    login(email, password);
  };

  const logout = async () => {
    await account.deleteSession("current");
    setLoggedInUser(null);
  };

  if (loggedInUser) {
    return (
      <div>
        <p>Logged in as {loggedInUser.name}</p>
        <button type="button" onClick={logout}>
          Logout
        </button>
      </div>
    );
  }

  return (
    <div>
      <p>Not logged in</p>
      <form>
        <input
          type="email"
          placeholder="Email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
        <input
          type="password"
          placeholder="Password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        />
        <input
          type="text"
          placeholder="Name"
          value={name}
          onChange={(e) => setName(e.target.value)}
        />
        <button type="button" onClick={() => login(email, password)}>
          Login
        </button>
        <button type="button" onClick={register}>
          Register
        </button>
      </form>
    </div>
  );
};

export default LoginPage;
```

## All Set

Run your project with `npm run dev` and open Localhost on Port 3000 in your browser.

Don't forget to add some CSS to suit your style.