---
title: An Introduction to The State Decoded
layout: default
---

# What It Does

The State Decoded is a program that takes structured legal data and generates a website, an API, and bulk downloads based on that legal data. It was designed for U.S. state and municipal legal codes, but it works just as nicely for regulations, contracts, even EULAs. It interfaces with court decisions, legislation, and arbitrary types of connected legal data.

# Who It’s For

Governments, open-government groups, lone data hackers, law libraries, or anybody else who wants to put legal data online.

# Getting a Copy of the Legal Code

Getting the laws or regulations in question can be a non-trivial challenge. It will almost certainly be more work than actually setting up The State Decoded.

The best-case scenario is that machine-readable, bulk data is available. This might be XML, JSON, or SGML of the legal code, provided directly by the government in question, or by a vendor on their behalf. This is enormously rare. The worst-case scenario is that the code is available only as printed materials, which would need to be scanned in and OCRed every time that the legal code is updated. The reality is that 95% of the time, the data is available as a middle ground, such as in RTF or Word files, or as HTML.

It’s quite likely that you’re going to be converting unstructured (or poorly structured) data into structured data. Maybe Word files, maybe screen-scraping HTML. In whatever your language of choice is—any programming language can handle it—you’ll be isolatiing, extracting, and storing some basic information about each law or regulation: its section number, some sort of structural parent number, an optional catch line or title, and the text of the law. ([See the XML format specification](xml-format.html) for detailed information about these fields.) If possible, develop this on a collaborative platform like GitHub and broadcast to others what you’re doing, because that will allow other people to assist you.

*Before you screen-scrape any HTML, make sure that you read the website’s terms of service!* If you are prohibited from duplicating the contents of that site, then proceed no further without consulting an attorney.

The best way to start obtaining bulk legal data is almost always to contact the government in question, after identifying which agency or department is the keeper of the code. It is nearly always best to approach with an open mind, planning to build a relationship with the employees at that education, educate them about what you’re doing and why it’s important, and expect to learn a great deal from them about how the legal code works. (If you aren’t an attorney with deep experience with the legal code in question, you almost certainly do not actually understand how it’s structured, how it’s maintained, or its quirks.) *Do not approach them by citing FOIA or otherwise demanding data with an official-sounding request.* This is often perceived as aggressive, and will not earn you any friends in that government. Be friendly! Talk to them about how you can promote their hard work on the code by making it more widely available, and explore how you can make their job easier. For instance, maybe they’ve been wanting an XML version of their legal code, and the parser you’re building for their Word files could be really helpful to them. These folks are underpaid, their work is underfunded, nobody ever tells them “I’m a big fan of your work.” In all likelihood, *you* are a fan of their work. Say so.

The worst-case scenario is that you’d need to issue a FOIA request to get the data that you need, and re-issue that request to get an updated copy as often as it’s updated. This is a last resort.

# You Must Provide Updates

If you are using The State Decoded for a legal code or for regulations, you must have an ongoing source of updates to those laws to keep your website current. This is an essential element without which you would be foolish to proceed. *It’s not enough to get a copy of the legal code once. You must have a method of obtaining regular updates as the code is updated.* This isn't a technical requirement for the software, but it’s a crucial practical prerequisite.

# A Stern Warning About Data Quality

By putting a legal code online, you are providing information that, inevitably, people will rely on. You’re implicitly making an agreement to the public that you will present accurate information and keep that information on your site up-to-date. You mustn’t start a site like this on a lark. You’re making an indefinite commitment to the public interest.
