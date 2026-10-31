# Unprotected Admin Functionality

**Platform:** PortSwigger Web Security Academy  
**Topic:** Access Control  
**Status:** Solved

## What was I trying to do?

The goal of this lab was to find the admin panel and delete the user carlos.

At first, I had no idea where the admin panel was. There was no visible link or button that took me to it, so I started exploring the website and looking for clues.

## How I approached the lab

### 1. First, I explored the website

I visited the different pages and paid attention to how the URLs changed.

I noticed that the URL was not the same on every page. This made me think that the application might be using different paths or parameters to load different content.

I also opened a few product pages and changed the product_id to a value that I thought would not exist.

**Why did I try this?**

I wanted to understand how the website handled product IDs and whether changing them would reveal anything interesting. It did not lead me to the admin panel, but it was part of my initial exploration.

### 2. I checked the account page

Next, I inspected the account page and tried a few username and password combinations.

I was checking whether there was any obvious way to discover or access the admin functionality through the account section.

This did not work either, so I moved on instead of spending too much time guessing credentials.

### 3. I looked for clues in robots.txt

Since I still had no idea where the admin panel was, I thought there might be a file that could reveal hidden or less obvious website paths.

That's when I decided to check:

    /robots.txt

I found this content:

    User-agent: *
    Disallow: /administrator-panel

This was the clue I was looking for!

**What does this mean?**

- User-agent: * means the rule applies to all web crawlers.
- Disallow: /administrator-panel asks crawlers not to visit that path.

In simple words, the website was telling search engine crawlers to stay away from that path, but it was also revealing the admin panel's URL.

One important thing I learned here: **robots.txt does not actually protect a page.** It only gives instructions to crawlers. Anyone can still try to open the path directly.

### 4. I tried opening the admin panel directly

After finding the path, I entered:

    /administrator-panel

I expected that the website might ask me to log in or deny access. But it opened the admin panel directly!

This was the actual security issue.

The application had an admin panel, but it was not checking whether the person accessing it was really an administrator.

This is an example of **Broken Access Control**.

In simple words, access control is what decides who is allowed to view or perform certain actions. Here, the website failed to restrict an admin-only page to admin users.

### 5. I deleted the requested user

Once I was inside the admin panel, I could see the list of users.

I found the user carlos and clicked the Delete option next to it.

The website confirmed that the user had been deleted, and PortSwigger showed the lab completion message.

**Lab solved.**

## What actually went wrong?

The main problem was not that the admin URL was visible in robots.txt.

The real problem was that the server allowed someone to access the admin panel without checking their permissions.

The website was relying on the admin URL being unknown instead of properly protecting the functionality.

This is sometimes confused with **security through obscurity**. It means assuming something is safe just because people do not know where to find it. But hiding a URL is not the same as securing it.

## What I learned from this lab

1. Exploring a website and observing URL patterns can help me understand how its pages work.
2. robots.txt can reveal paths that are not linked anywhere on the website.
3. A Disallow rule does not block users from opening that URL.
4. I should try accessing interesting endpoints directly and check whether the application properly restricts them.
5. Admin functionality must be protected on the server. Hiding the admin link or URL is not enough.

## How could this be prevented?

If I were securing an application like this, I would:

- Require users to log in before accessing the admin panel.
- Check the user's role and permissions on the server for every admin-only action.
- Make sure normal users cannot perform administrative actions just by entering a URL directly.
- Test admin routes using both logged-out and normal user accounts.

## My takeaway

This lab taught me that sometimes the useful clue is not hidden in complicated code or a complex payload. It can be sitting in a simple file like robots.txt.

I started by exploring pages, checking URL changes, and trying a few things that did not work. Eventually, one small clue helped me find the admin panel.

The bigger lesson for me was understanding the difference between **finding a hidden URL** and **finding an actual security vulnerability**. The URL helped me discover the panel, but the missing access-control check was what made the lab vulnerable.

## Lab Evidence

The following screenshots can be added as I organize the evidence:

- 01-robots-txt.png — the clue that revealed the admin path.
- 02-admin-panel.png — the admin panel opened directly.
- 03-lab-solved.png — the successful lab completion message.

## Scope

This exercise was performed in the intentionally vulnerable PortSwigger Web Security Academy lab. The user deletion was part of the lab objective and was not performed on a real website.
