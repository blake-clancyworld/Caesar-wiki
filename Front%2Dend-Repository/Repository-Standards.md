#Repository Standards

##Framework

* Our application is built using [NextJS](https://nextjs.org/) a popular framework for building server-rendered React applications. 

## TypeScript

* [TypeScript](https://www.typescriptlang.org/) is our primary programming language. 

## Front-End Component Library: Mantine

* [Mantine](https://mantine.dev/) is our front-end component library. Mantine is a popular open-source React library that provides a wide range of customizable and reusable UI components.

* We use Mantine to build the user interface of our application, taking advantage of its predefined components for common UI elements such as buttons, forms, and navigation. Mantine also includes utility functions such as a built in notification system and styles that we use to further customize the look and behavior of our components.

##Page Structure

* Our NextJS pages are broken down into two main parts: containers and components. Containers are responsible for fetching and organizing data, while components are responsible for rendering that data. This separation of concerns makes it easier to reason about our code and make changes without affecting unrelated parts of the application.

##Client-Side Data Fetching

* For client-side data fetching, we use the SWR library. SWR is a lightweight library that makes it easy to fetch data on the client side and keep it in sync with the server. It also includes features like caching and automatic refetching, which can improve the performance and user experience of our application. 

* In addition, SWR is maintained by Vercel, the same group that maintains NextJS so it works well with our framework.

* You can find more information about SWR in the [documentation](https://swr.vercel.app/docs/data-fetching)

##Server-Side Data Fetching

* For pages that require server-side data fetching, we use the getServerSideProps lifecycle method. This method allows us to fetch data on the server and pass it to the client, where it can be rendered in the browser. Using getServerSideProps can improve the performance of our application, especially for pages that require data that is expensive to fetch or compute on the client side.

##Authentication
* In our NextJS application, we use:

  1. [NextAuth](https://next-auth.js.org/) to handle authentication, authorization, and to handle the session management for authenticated users.
  2. [AWS Cognito](https://aws.amazon.com/cognito/) to maintain our user pool and generate authentication tokens. 

  3. [amazon-cognito-identity-js](https://www.npmjs.com/package/amazon-cognito-identity-js) to interact with AWS Cognito and implement custom forms for user sign-in and sign-up.

* By default, NextAuth provides a built-in `CognitoProvider` that can be used to easily integrate with AWS Cognito. However, in our case, it was desirable to create custom forms for handling user authentication. In these cases, `amazon-cognito-identity-js` can be used to manage the communication with AWS Cognito and NextAuth can be used to manage the user session.

* To use `amazon-cognito-identity-js` in our application, we follow the guidelines provided in the library's documentation. This includes setting up an instance of the `CognitoUserPool` class and using methods like `signUp` and `authenticateUser` to handle user sign-up and sign-in respectively. NextAuth's `signIn` and `signOut` methods can then be used to manage the user session.
