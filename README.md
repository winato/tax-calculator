# Tax Calculator

A lightweight Vue 3 app for estimating Dutch RSU payouts. Production [link](https://winato.github.io/tax-calculator/)

## What it does

This calculator helps users estimate the net value of Restricted Stock Units (RSUs) in the Netherlands by:
- calculating gross RSU value from quantity and unit price
- applying a fixed Dutch tax rate
- showing taxable amount with or without the 30% ruling
- formatting results as EUR currency

## Key features

- clean, responsive UI
- native Vue 3 composition API
- currency formatting for EUR results
- simple and fast RSU payout calculation

## Project structure

- `src/main.js` — app bootstrap
- `src/App.vue` — main layout
- `src/components/Calculator.vue` — calculator UI and logic

## Setup and development

```bash
yarn install
yarn serve
```

Open the app at `http://localhost:8080` by default.

## Build for production

```bash
yarn build
```

## Linting

```bash
yarn lint
```

## Notes

- The app currently uses a fixed tax rate of `39.21%`.
- The 30% ruling is applied as a simple reduction on taxable income.
- Use `nvm use 16` if your local Node version differs from the project requirements.
