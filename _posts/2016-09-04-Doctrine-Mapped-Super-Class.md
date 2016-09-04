---
layout: post
title: Doctrine Mapped Super Class 
---

Think that, there is a OAuth Module for an API and you try to create user model 
for this module but you have already create another user model, I mention as 
`BaseUser`, for your system. You can use directly `BaseUser` model. In my view, 
this is wrong for modular system. You should extend `BaseUser` for your OAuth 
module. In this scenario, you can easily extend your `BaseUser` model and 
add external methods for OAuth systems. 

Lets create a example for this situation:

```
/**
 * @ORM\Entity()
 * @ORM\Table(name="user")
 */
class BaseUser
{

    /**
     * @var integer $id
     *
     * @ORM\Id
     * @ORM\Column(name="id", type="integer")
     * @ORM\GeneratedValue(strategy="IDENTITY")
     */
    protected $id;

    /**
     * @var string
     * @ORM\Column(name="username", type="string", length=100)
     */
    protected $username;

    /**
     * Password
     *
     * @ORM\Column(name="password", type="string", length=150)
     * @var string
     */
    protected $password;
}
```

Now, lets create an OAuth model: 

```
use League\OAuth2\Server\Entities\UserEntityInterface;
use YourModule\Entity\User as BaseUser;

/**
 * Class User
 */
class User extends BaseUser implements UserEntityInterface
{
    /**
     * Return the user's identifier. This method required extenal OAuth module 
     * but there is no getIdentifier method on my BaseUser model
     *
     * @return string
     */
    public function getIdentifier()
    {
        return $this->getUsername();
    }
}
```

Now, we update our schema for doctrine. 

```
php vendor/bin/doctrine orm:schema-tool:update --dump-sql
```

The response will be weired. Doctrine gives you some errors about super class.

```
  [Doctrine\ORM\Mapping\MappingException]
  Class "OAuth2Server\Entity\User" sub class of "YourModule\Entity\User" is not a valid entity or mapped
  super class.
```

Doctrine say us that, there is a class and extends an Entity. Do something to 
solve this problem because I dont know how to use this class as an entity or 
what? There is to way to solve this problem. First one is: 

Add your OAuth's User as basicly `Entity`

```
/**
 * Class User
 *
 * @ORM\Entity()
 */
class User extends BaseUser implements UserEntityInterface
{
  // ...
}
```

In this scenario, Doctrine will create us a new `User` table. We dont want it.
We want to use our base db table as our OAuth user table. We should use 
`MappedSuperclass` feature of Doctrine like this:

```
/**
 * Class User
 *
 * @ORM\MappedSuperclass
 */
class User extends BaseUser implements UserEntityInterface
{
  // ...
}
```

Now, we can use User class as an entity and this entity also refer our BaseUser
entity and our base user table. 