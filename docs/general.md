# General ⚡

[Return to Table of Contents](../README.md)

## **Intro**  

Hi there! Everybody somehow somewhere faced troubles asking themselves how should I name it?, where I have to place it? How should I build it? and a lot more. So in here we will see some rules and features how to keep our project safe and sound, and in order. This is step by step guide that can help you understand the basics of building and supporting projects in our company - CGS-team. I should mention that we will talk in scope of Typescript and also React, but never the less other developers in other technologies as well can find here something useful 🚀🚀🚀

## **Naming**

### File names should be in kebab-case  ✅

> Our team was inspired by Angular in scope of file/directory naming. For those purposes we always use kebab-case .   
> It helps to name your files in much more clear way.  

```
history.type.ts
calculate-outcome.util.ts
links.const.ts
manager.controller.ts
user.service.ts
post.entity.ts
```

### Use suffixes for your file names ✅

> From time to time it's a bit hard to navigate in haystack of open files.  
> You know we have an answer for that problem - use suffixes to make file search and navigation easier.  

> Frontend:  
```
history.component.ts
calculate-outcome.container.ts
roles.util.ts
links.const.ts
manager.styled.ts
user.service.ts
post.provider.ts
history.type.ts
```  

> Backend:  
```
history.component.ts
calculate-outcome.container.ts
roles.util.ts
links.const.ts
manager.controller.ts
user.service.ts
post.entity.ts
```

> This is the list of allowed suffixes which you can use for file naming.  
> Our git hooks configured to prevent pushing files without suffixes, so be aware )  

### Use suffixes for your folder names ✅

> It's obvious but still, please, use `kebab-case` for your folders too to keep everything consistent.  

```
- modules
-- account-tracking
-- user-management
```


## **Folder structure**

### Use modular approach for large projects and medium projects ✅  

> If project which you are working on has opportunities to grow and scale it is time to think about modules.
> Modules helps you to keep your code clear and migrate easy to Micro Service architecture in future.
> We usually use the following structure:  


> Frontend:  

```
src
- modules
-- user
--- components
---- user-input.component.ts
---- user-avatar.component.ts
---- user-slider.component.ts
---- index.ts
---
--- containers
---- user-page.container.ts
---- user-details-page.container.ts
---- index.ts
---
--- services
---- user-auth.service.ts
---- user-stats.service.ts
---- index.ts
---
--- utils
---- crop-user-avatar.util.ts
---- index.ts
---
--- types
---- user.types.ts
---- index.ts
---
--
-

```


> Backend:  

```
src
- modules
-- user
--- controllers
---- user.controller.ts
---- index.ts
---
--- services
---- user-auth.service.ts
---- user-stats.service.ts
---- index.ts
---
--- utils
---- crop-user-avatar.util.ts
---- index.ts
---
--- types
---- user.types.ts
---- index.ts
---
--
-
```

## **Tools**

### Use quality check tools ✅  

> We have an automated way to control everything that was described above - Git Hooks.  
> For our all new projects we use them, current version of hooks supports frontend, backend and smart contracts.  
> Are you interested in ?  
> Ask @Danyyl Kuchkov or @Timur Banax - to tell you more about that.  

## **Useful links**

[**Our inspiration was taken from that**](https://angular.io/guide/styleguide)

