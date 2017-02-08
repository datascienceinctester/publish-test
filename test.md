# DataScience Cloud Platform

This project provides the core of the Monolithic DataScience Cloud Product.

- [Installation](#installation)
- [Usage](#usage)
- [Development](#development)
- [Database Migrations](#database-migrations)
- [Additional Information](#additional-information)
- [Troubleshooting](#troubleshooting)

## Installation

### Dependencies:

- NodeJS
- PostgreSQL

### Setup (OS X):
1. Install NodeJS on your Mac with `brew install node`
1. Install yarn with `brew install yarn`
1. Install flyway with `brew install flyway`
1. Clone the platform repo with `git clone https://github.com/datascienceinc/platform.git`
1. Install Node dependenies with `yarn install --ignore-engines`
1. Download [Postgres.app](https://postgresapp.com) and drag the file into your `~/Applications` directory.
1. Double click the `Postgres.app` icon to inititialize the PostgreSQL server.
1. Configure your $PATH to use the included command line tools
	```
	sudo mkdir -p /etc/paths.d &&
	echo /Applications/Postgres.app/Contents/Versions/latest/bin | sudo tee /etc/paths.d/postgresapp

	```

1. Install `nbconvert` command line tool. More on installing `pip` can be found [here](https://pip.pypa.io/en/stable/installing/).

   `pip install nbconvert jupyter_client`

1. Create the database user

	`createuser dsclouduser --superuser --pwprompt` and you will be prompted for the password. Use `dscloudpassword`.

1. Create the database

	`createdb platform --owner=dsclouduser`

1. Run the SQL migrations

	`npm run db:migrate:latest`

1. `npm run dev` to bring up the platform application

1. Run this CURL command to see if the API and database are running:

	`curl -X POST http://localhost:8000/api/auth/login --data '{"email": "admin@demo.com", "password": "pass"}' -H "content-type: application/json"`

1. Go to [http://localhost:8000](http://localhost:8000) and login with `admin@demo.com / pass` to verify the front-end is working correctly.

#### React-Storybook
Browse to http://localhost:9001

#### Web Application
Browse to http://localhost:8000

#### API
Browse to http://localhost:8000/api/status

## Development

### Adding an NPM Module

We use [Yarn](https://yarnpkg.com/en/docs/cli/) to manage our npm dependencies. To add a module from npm, `yarn add <module-name>` which will add the dependency to the
package.json and the `yarn.lock` file. For more information about adding dependencies via Yarn, see the [Yarn docs](https://yarnpkg.com/en/docs/cli/add).

### Running Tests

#### Unit Tests

    npm run test:unit

#### Integration Tests

    npm run test:integration

## Database Migrations

You can use some convenience scripts to run the migrations:

`npm run db:migrate:info` will show the state of your database schema
`npm run db:migrate:clean` will drop all tables
`npm run db:migrate:latest` will run  migrations that need to be run

### Adding a migration script

To add a database migration add a `V{timestamp}_{short_description}.sql` file to the `/sql` folder in the project root. To get the timestamp run this command in the JavaScript console of your browser `Math.floor(new Date().getTime() / 1000)`

The file should just include SQL statements, typically `CREATE`, `DELETE` and `ALTER` statements.  If you're adding `INSERT`, `UPDATE`, or `DELETE` statements its worth discussing first.

### Running database migrations

`psql -d platform -U dsclouduser < filename.sql`

## Additional Information

### Export NPM Token

`NPM_TOKEN` is what allows the docker container to authenticate with `npm` for private modules. It is exposed as a build argument, which is cleaned up before the container is finished building.

    # Login to NPM
    npm login

    # Open credentials file. Copy everything after `authToken=`. This is your NPM_TOKEN.
    (vi | vim | subl | atom | nano ) /Users/[username]/.npmrc


    # Paste the following in your bashrc, bash_profile, or equivalent
    export NPM_TOKEN=[token-from-npmrc]

    # Reload or restart your terminal.
    source ~/.bashrc
