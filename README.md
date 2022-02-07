# Frontend Developer Test

One of our clients has asked us for help with the development of a web application that manages the process of selecting a room in their hotel. It already has an initial development but at THN we think it can be improved.

Our client has told us that the project must meet the following objectives:
- Checkin and checkout must show the current date and tomorrow's date.
- When the "Modify" button is clicked, the necessary information must be updated in the "Summary" section.
- When you select one of the 3 hotels in the list, the information in the "Summary" section must be updated.
- When the "Save" button is clicked, all selected information should be saved locally. This means that when you refresh the browser tab, the current data will be kept.
- When the `promo_code` parameter is passed by URL, it should apply a discount on the final price. For example, `dummyurl.com?promo_code=10`, will apply a 10% discount both in the Summary section and in each block of the hotel.

Nice to have:
- The web application must be adapted to mobile devices.
- Maintaining the current development framework (Vue)

## Important:
There is no limit to the things you can change. Not only add missing functionality but also improve the use of the framework, project architecture, bug fixes, etc... so feel free. The aim is that you should deliver the project as you would deliver it.

---

## Project Setup

```sh
yarn install
```

### Compile and Hot-Reload for Development

```sh
yarn run dev
```

### Type-Check, Compile and Minify for Production

```sh
yarn run build
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```
