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

Important: this buildpack only prepares the monorepo workspace (installs dependencies at the repo root
while ignoring lifecycle scripts). It does NOT run the app build. To ensure Node is available when the
build runs, add `heroku/nodejs` as the *second* buildpack. Recommended order:

```sh
heroku buildpacks:clear -a <your-app>
heroku buildpacks:add https://github.com/<your-username>/heroku-buildpack-pnpm-monorepo.git -a <your-app>
heroku buildpacks:add heroku/nodejs -a <your-aspp>
heroku config:set APP_DIR=apps/web -a <your-app>
```

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
