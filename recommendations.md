---
title: API Usage recommendations
---
# Overview

This page describes the best practices and recommendations you should follow when using Geo APIs.

# Best practives

__IMPORTANT__ - To ensure the best stability of your service, you should follow these best practices when implementing clients:

- Your clients should not rely on properties that are not documented in this specification as such properties could be changed or removed _without notice_.
- Your clients should not assume that the properties described in this specifications are the only ones that will be returned by the API. Not documented or additional properties will be returned in future version of this API _without notification_.
- Your clients should not rely on or map properties that are not usefull to them. Doing so will ensure that your clients are not impacted by API changes on properties that are not used by them.


