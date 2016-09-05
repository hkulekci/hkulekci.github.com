---
layout: post
title: Doctrine Mapped Super Class 
---

If you want to extend an entity class and you dont want to create a new table on
your db, you need to use MappedSuperClass. 

For example, you want to use OAuth2 of PHP League on your system and you 
should create user model to implement server. But you have already create 
another user model, I mention as `BaseUser`, before for your own system. 
Of course You can use directly `BaseUser` model. In my view, this is wrong for 
modular system for two reason. The first reason is you can not seperate your 
server module from your base module. The second reason is, your `BaseUser` model
will contain unnecessary methods about OAuth2. So, you should extend `BaseUser` 
for your OAuth module. In the first scenario, you can easily extend your 
`BaseUser` model and add external methods for OAuth2 systems. 

Lets create a sample User Entity for this situation:

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
 * Class OAuth2User
 */
class OAuth2User extends BaseUser implements UserEntityInterface
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

Run the following commands to update our schema for doctrine: 

```
php vendor/bin/doctrine orm:schema-tool:update --dump-sql
```

Lets look what the response will be. Doctrine gives you some errors about super 
class:

```
  [Doctrine\ORM\Mapping\MappingException]
  Class "OAuth2Server\Entity\User" sub class of "YourModule\Entity\User" is not a valid entity or mapped
  super class.
```

Doctrine say us, there is a class and it extends an Entity. Do something to 
solve this problem because I dont know how to use this class as an entity or 
what? There are some solution to solve this problem. First one is: 

Mark your OAuth2User as basicly `Entity` class

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

In this scenario, Doctrine will create us a new `User` table. But we don't want 
it. We want to use our `BaseUser` table as our OAuth2User table. We want to 
extend our DB table also, whe we extends our class. You should use 
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