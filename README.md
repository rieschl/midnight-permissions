[![Build Status](https://travis-ci.org/MidnightDesign/midnight-permissions.svg?branch=master)](https://travis-ci.org/MidnightDesign/midnight-permissions)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/MidnightDesign/midnight-permissions/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/MidnightDesign/midnight-permissions/?branch=master)

midnight/permissions
====================

ACL and RBAC weren't the permission models I was looking for. So I wrote my own.

## Installation

Install `midnight/permissions` via Composer.

## Usage

You need a [Container](https://github.com/container-interop/container-interop) that can provide your `PermissionService`
with permissions. In this example, we're going to use `league/container`, but any container implementing 
`Interop\Container\ContainerInterface` will work.

For brevity, the user and resource are arrays in this example. Most likely, they will be objects any real-world project.

```php
class CanDoSomething implements Midnight\Permissions\PermissionInterface
{
    public function isAllowed($user = null, $resource = null) {
        return $user === $resource['owner'];
    }
}

$container = new League\Container\Container;
$container->add('can_do_something', CanDoSomething::class);

$permissionService = new Midnight\PermissionService\PermissionService($container);

$permissionService->isAllowed('Rudolph', 'can_do_something', ['owner' => 'Rudolph']); // true
$permissionService->isAllowed('Rudolph', 'can_do_something', ['owner' => 'Christoph']); // false
```

## Zend Framework 2 Module

There's also a ZF2 module that will let you access the `PermissionService` via the Service Manager, add permissions via 
the config and give you view helpers and controller plugins. It's called 
[midnight/permissions-module](https://github.com/MidnightDesign/midnight-permissions-module).
