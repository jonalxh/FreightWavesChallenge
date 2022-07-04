# FreightWaves Senior Code Challenge

This challenge was made with the following stack:

- Vue JS 3.
- Quasar Framework (Vue based).
- Axios.
- ES6.
- Vue Router.
- Eslint.
- Prettier.
- SCSS.

I have chosen to build this project on top of `Vue JS 3` because it is the last version and can be worked with `Vite`, a very fast tool for hot-code reloading.

`Quasar Framework` allowed me to stylish the app very fast, with beautiful material design components like buttons, navbar, sidebar, dialog, inputs and icons. There are a lot of alternatives, like `Bootstrap Vue` (does not have Vue 3 support), `Vue Material` or `Vuetify`, I preferred `Quasar` because it has a very wide community supporting the project, I have been using it since its first versions and I have seen how it has evolved.

I decided to create a few `components` for the table to show you how I make the relations between parent and child components and tried to show the slots functionality too.
The form handling uses a recursive way to dynamically create inputs from an object, it allows us to save a lot of code, there are different ways of doing it and needs to be improved for different data types.

The `API` is initialized in `boot/axios.js` and it is just an instance of `Axios` with a defined base url.

The details ad edit views were created as a modal to avoid user redirection and keep the user in the same context, [here](https://uxplanet.org/modal-vs-page-a-decision-making-framework-34453e911129) is a very good article talking about it.

Having more time we can improve the project a lot with the following:

- Infinite scroll to load more records when user scrolls.
- Add more routes.
- Add an authentication method like firebase auth.
- A map plugin for the user to choose their address.
- A companies select to change the user company.
- Improve the table components for better performance.

## Install the dependencies

```bash
npm install
```

### Start the app in development mode (hot-code reloading, error reporting, etc.)

```bash
quasar dev
```

### Lint the files

```bash
npm run lint
```

### Format the files

```bash
npm run format
```

### Build the app for production

```bash
quasar build
```
