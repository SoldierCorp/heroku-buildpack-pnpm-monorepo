# Heroku Buildpack: pnpm Monorepo

A simple Heroku buildpack for deploying a pnpm monorepo. Supports selecting the app to build by folder path (`APP_DIR`) or by package.json name (`APP_NAME`).

## Usage

1. Push this folder to a public GitHub repo (e.g., `heroku-buildpack-pnpm-monorepo`).
2. Add the buildpack to your Heroku app:
   ```sh
   heroku buildpacks:clear -a <your-app>
   heroku buildpacks:add https://github.com/<your-username>/heroku-buildpack-pnpm-monorepo.git -a <your-app>
   ```
3. Set one of these env vars:
   - `APP_DIR=apps/web` (or your app folder)
   - `APP_NAME=@app/web` (or your package.json name)

## How it works
- Installs pnpm
- Installs dependencies from the monorepo root
- Builds the app in the specified directory
- Sets the release phase to `pnpm run start` in your app

## Example
```sh
heroku config:set APP_DIR=apps/web -a <your-app>
# or
heroku config:set APP_NAME=@app/web -a <your-app>
```

## Requirements
- Your app must have a `build` and `start` script in its package.json
- Your monorepo must use pnpm and a single root lockfile
