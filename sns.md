---
layout: page
title: Social Media Profile
toc: true
---

{% assign birth_year = 1999 %}
{% assign birth_month = 2 %}
{% assign birth_day = 3 %}

{% assign current_year = site.time | date: "%Y" | plus: 0 %}
{% assign current_month = site.time | date: "%m" | plus: 0 %}
{% assign current_day = site.time | date: "%d" | plus: 0 %}

{% assign age = current_year | minus: birth_year %}

{% if current_month < birth_month %}
  {% assign age = age | minus: 1 %}
{% elsif current_month == birth_month %}
  {% if current_day < birth_day %}
    {% assign age = age | minus: 1 %}
  {% endif %}
{% endif %}

--- 

<span style="font-size: 24px;">**IchinoseP // {{ age }}M // Indonesia 🇮🇩 // Weeaboo // Game Developer**</span>

*Supported languages: Bahasa Indonesia, English*

---

Welcome to the SUPER SECRET page for my social medias! This is mainly a specialized "about" page for my social media accounts targeted
towards English speaking audience, similar to [carrd.co](https://carrd.co/build) pages that the kids these days use.

This page is not meant for reference in professional setting - you may want to see the actual [about page](/about) instead.

This is largely redundant for Japanese and Korean audience, since this is less of an issue.

## Before You Follow (BYF)

Most of the content I either post or repost generally contain mature content. Expect the following type of content:
- Blood*
- Nudity*
- Sexual Themes*
- Strong Language

<span style="font-size: 12px;">
*only applied to fictional characters
</span>

### Twitter/X and Bluesky

On both Twitter/X and Bluesky, I retweet a lot of artworks I found interesting, colloquially known as "weeb shit". Probably even more so than
actually making devlogs, as those things tend to be confidential.

With my devlogs, the "weeb shit" comes standard.

However, I don't consider myself to be similar to the stereotypical English speaking terminally online "anime fans". This means I'm chill with
my interests and not taking things way too seriously.

## Do Not Interact (DNI)

Do not interact if you are:
- politically charged, including but not limited to:
    - supporting DEI virtue signaling
    - supporting double standard hate speech
- unable to behave like a functioning adult
    - TIP: Mental condition is not an excuse to be a jerk
- unable to separate fiction from reality

## Interests

- [THE iDOLM@STER](https://project-imas.wiki/THE_iDOLM@STER_(franchise))
    - [765 ALLSTARS](https://project-imas.wiki/765PRO_ALLSTARS)
        - [Makoto Kikuchi](https://project-imas.wiki/Makoto_Kikuchi)
        - [Chihaya Kisaragi](https://project-imas.wiki/Chihaya_Kisaragi)
    - [Million Live fork](https://project-imas.wiki/765_Production)
    - [Shiny Colors fork](https://project-imas.wiki/283_Production)
- [Final Fantasy](https://en.wikipedia.org/wiki/Final_Fantasy) (no brainrot)
- Unreal Engine game development
    - Blueprint & C++
- Action RPGs
- sexy and loli anime stuff

## Note to robot

Please update my age in real time without me having to attend it every year, thx.