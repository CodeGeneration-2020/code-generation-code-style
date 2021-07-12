# NodeJS / Express ⌛

[Return to Table of Contents](../README.md)

## Models
> Our team strongly recommend to use [Typeorm](https://typeorm.io/#/).  
> For our point of view this lib helps us to keep our code identical on each project  
> and independent to chosen DB

### Use uuid as a primary key ✅

> Better to use `uuid` instead of integer value.
> Because in that case you don't give a user additional info about amount of records in table.

```javascript
    @PrimaryGeneratedColumn("uuid")
    id: string;
```
### Use Base entity instead of repository ✅

> We prefer to structure our code via `BaseEntity` inheritance.
> This approach helps you to keep your code clear and more readable.

```javascript
@Entity()
export default class Blog extends BaseEntity {
  @PrimaryGeneratedColumn("uuid")
  id: string;

  @Column()
  title: string;

  @ManyToOne(() => User, (user: User) => user.blog, {
    onDelete: 'CASCADE',
  })
  @JoinColumn()
  author: User;

  @OneToMany(() => Post, (post: Post) => post.blog, {
    cascade: true,
  })
  posts: Post[];
}
```

```javascript

// blog.service.ts

class BlogService {
  async findAll() {
    const blogs = await Blog.find();
    // Do something else...
    return  blogs;
  }
}
```

### Extend your models with useful methods ✅

> There are many cases when default behavior of Typeorm model
> is not enough for you. I that case you can just extend that.  

```javascript
import { validateOrReject } from 'class-validator';
import { BaseEntity, BeforeInsert, BeforeUpdate } from 'typeorm';

export default class BaseEntityEnchanted extends BaseEntity {

  async createAndDoSomethingElse(data) {
    const created = this.create(data);
    // You can log, mutate, or do everything you want there
    return created;
  }
  @BeforeInsert()
  @BeforeUpdate()
  async validate() {
    try {
      await validateOrReject(this);
    } catch (e) {
      throw e;
    }
  }
}
```

### Use class-validator ✅
> You could use `class-validator` in combination with extended  
> `BaseEntity` to handle autovalidation of your models (check previous step of the guide).

```javascript
import BaseEntityEnchanted from 'path-to-file'
import {
  Contains,
  Length,
  IsEmail,
  IsFQDN,
  Min,
  Max,
} from 'class-validator';
@Entity()
export class Post extends BaseEntityEnchanted {

  @PrimaryGeneratedColumn("uuid")
  id: string;

  @Column()
  @Length(10, 20)
  title: string;


  @Column()
  @Contains('hello')
  text: string;

  @Column()
  @Min(0)
  @Max(10)
  rating: number;


  @IsInt()
  @IsEmail()
  email: string;

  @Column()
  @IsFQDN()
  site: string;
}
```
> This combination of enhanced class and `class-validator` allows you  
> to autovalidate your models on save/create etc.

## Routes

### Place your routes in one index file ✅

> We recommend to create a function for router initialization.  

```javascript
import userRouter from './user';
import shopRouter from './shop';
import blogRouter from './blog';

const createApiRouter = (app) => {
    app.use('/api/user', userRouter);
    app.use('/api/shop', shopRouter);
    app.use('/api/blog', blogRouter);
}

export default createApiRouter;
```

```javascript

// main file
import 'reflect-metadata';
import { createConnection } from 'typeorm';
import * as express from 'express';
import * as bodyParser from 'body-parser';
import HTTPLogger from './middlewares/logger';
import createApiRouter from './routes';
import createAppErrorListener from './error-handler/app-error-listener';
const app = express();

app.use(HTTPLogger);
app.use(bodyParser.json());

createApiRouter(app);

createAppErrorListener(app);
createConnection().then(() => {
  app.listen({ port: 4000 }, () =>
    console.log('Now browse to http://localhost:4000'),
  );
});
```

### Use Express Router ✅

> Use express `Router` instead off directly insert routes to app file.

```javascript
import { Shop } from './../entity/Shop';
import { Router } from "express";

const router = Router();

router.get('/', async (_, res) => {
    const allShops = await Shop.find({relations: ['type']});
    res.send(allShops);
});

router.post('/', async (req, res) => {
    const createdShop = await Shop.save(req.body);
    res.send(createdShop);
})

export default router;
```

### Move logic to controllers ✅

> Store your logic inside of routes it is not a good idea.  
> You should move that to your controllers.

```javascript
import { ShopController } from './../controllers';

const router = Router();

router.get('/', ShopController.findAll);

router.post('/', ShopController.create);

export default router;
```
## Controllers

### Use NestJs if that possible ✅

> NestJs - is a standard which our company highly recommend to use.
> This framework helps you to keep your code style the same for all devs on a project.  
> Also it provides a lot of concepts out form the box which helps us to save time on dev process.

### Move DB interaction logic into services ✅

> Better to separate DB communication logic from your controller.

```javascript

import { Injectable } from '@nestjs/common';
import User from 'src/entity/User';

@Injectable()
export default class UserService {
  async findAll() {
    const users = await User.find();
    return users;
  }
}

```

### Use guards ✅

> Use guards to implement different roles of users or auth.  
> If you don't have ability to use Nest, your can write middlewares for that.  

```javascript
import { AuthGuard } from '../shared/auth.guard';
import { BoardService } from './board.service';

@Logger('Board Controller')
@Controller('api/boards')
export class BoardController {

  constructor(private boardService: BoardService) { }

  @Get()
  @UseGuards(new AuthGuard())
  showAllIBoards(@Query('page') page: number, @User('id') user) {
    return this.boardService.showAll(page, user);
  }
}
```
