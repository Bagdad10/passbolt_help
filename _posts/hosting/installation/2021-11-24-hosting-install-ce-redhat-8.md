---
title: Install Passbolt CE on Red Hat 8
date: 2021-11-24 00:00:00 Z
description: How to install Passbolt CE on
card_title: Red Hat 8
card_teaser: Install passbolt CE on Red Hat
card_position: 10
icon: fa-redhat
categories: [hosting,install,ce]
sidebar: hosting
layout: default
slug: redhat
permalink: /:categories/:slug.html
---

{% assign product = 'ce' %}
{% assign distribution = 'redhat' %}
{% assign distributionVersion = '8' %}
{% assign distributionSlug = 'redhat' %}
{% assign distributionLabel = 'Red Hat' %}
{% assign distributionPackage = 'dnf' %}

{% include layout/row_start.html %}
{% include layout/col_start.html column="7" %}
distributionPackage
{% include hosting/install/packages/debian/install-debian-package.md %}

{% include date/updated.html %}

{% include layout/col_end.html %}
{% include layout/col_start.html column="4 last push1" %}

{% include aside/ce-install-community-forum-cta.md %}

{% include aside/ce-stay-up-to-date.md %}

{% include layout/row_end.html %}
