---
layout: post
title: Java - Encapsulation 封裝
category: java
tags: [java]
comments: true
toc: true
---

Data (or properties or attributes) and behaviour (the "methods" that operate on the data) are encapsulated into objects; moreover, 
the data is *hidden* from the external world.

## Access Modifier:

<table style="text-align: center">
    <thead>
        <tr>
            <th></th>
            <th>class</th>
            <th>package</th>
            <th>subclass<br>(same pkg)</th>
            <th>subclass<br>(diff pkg)</th>
            <th>world</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>public</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
        </tr>
        <tr>
            <td>protected</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td></td>
        </tr>
        <tr>
            <td>default</td>
            <td>+</td>
            <td>+</td>
            <td>+</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>private</td>
            <td>+</td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
</table>

---