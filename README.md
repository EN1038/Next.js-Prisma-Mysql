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

