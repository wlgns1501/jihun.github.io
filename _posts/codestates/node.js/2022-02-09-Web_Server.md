---
layout: single
title: "SEB Section 2 Web Server"
categories: codestates-node.js
tag: [JS/Node]
toc: true
---

# Cors(Cross Origin Resource Sharing)

옛날에는 서버에서 클라이언트 파일을 갖고 있어, 유저가 서버에 요청을 하면 서버에 있는 클라이언트를 유저가 받아가서 그 클라이언트에서 서버와 통신을 하거나, 클라이언트의 static 파일에 있는 데이터를 유저가 일방적으로 보는 방식으로 했다.
이때 굳이 서버는 유저가 서버에 요청을 하는 것을 의심의 여지 없이 받아 줬는데, 그 이유는 origin이 같아서 받아 줬었다.

하지만 최근에 들어서 싱글 페이지 어플리케이션과 웹이 고도화 되어서, 리서버에서만 요청을 하는게 아니라 여러 api에 리소스를 활용할 필요가 생겼다. 같은 서버에서 same origin으로 리소스를 받아 올 수 있었지만, 여러 api에 요청을 하려면 다른 origin, cross origin으로 리소스를 요청을 해야 한다.

보안 상의 이유로, 브라우저들은 cross-origin HTTP 요청을 제한 한다고 한다. 하지만 개발자들이 cross-domain 요청을 가능하게 해달라해서, 브라우저들은 서버가 허용한 범위내에서 cross origin 요청을 허용하고 있다.
