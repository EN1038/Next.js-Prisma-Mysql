# Next.js-Prisma-Mysql
The next.js use prisma connect Mysql 10/23/2024

## Install Next.js
Use in cmd npx create-next-app@latest

## Install Prisma && MySQL Client
Start install Prisma && MySQL Client be important MySQL 
use command " npm install prisma @prisma/client mysql2 "
After that install Prisma CLI
use command " npx prisma init "

## Setting file prisma/schema.prisma
After running the npx prisma init command, there will be a schema.prisma file. It is created in the prisma folder in your project. Edit this file to set up a connection to MySQL

prisma

    datasource db {
      provider = "mysql"
      url      = env("DATABASE_URL")
    }
    
    generator client {
      provider = "prisma-client-js"
    }

So go to the .env file to the MySQL technical section.

    DATABASE_URL="mysql://USER:PASSWORD@HOST:PORT/DATABASE"

USER: nameuser MySQL ของคุณ (root)
PASSWORD: password MySQL ()
HOST: address server MySQL (localhost)
PORT: port MySQL (The default value is 3306)
DATABASE: The name of the database you want to connect to.

This DATABASE_URL of Laragon

    DATABASE_URL="mysql://root:@localhost:3306/test-prisma"

## Create model in schema.prisma
Next you will need to define the model for your technical steps such as the model users

    model User {
      id        Int      @id @default(autoincrement())
      name      String
      email     String   @unique
      password  String
      createdAt DateTime @default(now())
    }

## Migrate Database
After the model is created, migrate it to create the tables in the database

    npx prisma migrate dev --name init

This command creates a table based on the model defined in schema.prisma and connect to MySQL

## Create Prisma Client
After migrating, run the following command to create the Prisma Client used to connect to the database in Next.js

    npx prisma generate

## Using Prisma in Next.js
Create or edit the lib folder and create the prisma.js file to set up the Prisma Client.
    
    // lib/prisma.js
    import { PrismaClient } from '@prisma/client';
    
    const prisma = new PrismaClient();
    
    export default prisma;

## Extracting data from the database
Example of retrieving data from the User table in the file app/api/users/route.js

        // app/api/users/route.js
    import prisma from "@/lib/prisma";
    
    export async function GET() {
      try {
        const users = await prisma.user.findMany();
        return new Response(JSON.stringify(users), {
          status: 200,
          headers: {
            'Content-Type': 'application/json',
          },
        });
      } catch (error) {
        return new Response('Error fetching users', { status: 500 });
      }
    }

## Insert new data
Example insert users table in users

        // app/api/users/route.js
    import prisma from "@/lib/prisma";
    
    export async function POST(request) {
      try {
        const { name, email, password } = await request.json();
        const newUser = await prisma.user.create({
          data: { name, email, password },
        });
        return new Response(JSON.stringify(newUser), {
          status: 201,
          headers: {
            'Content-Type': 'application/json',
          },
        });
      } catch (error) {
        return new Response('Error creating user', { status: 500 });
      }
    }

## Use Prisma Studio for check datas

    npx prisma studio

Prisma Studio opens a browser-based UI that lets you view and edit database data as easily as using phpMyAdmin.





