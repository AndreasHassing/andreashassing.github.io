---
layout: page
title: About
permalink: /about/
---

Hi there,

My name is Andreas Bjørn Hassing, I'm a <span id="my-age"><noscript>not too</noscript></span> old software engineer from Denmark, currently working for Microsoft in Copenhagen (also in Denmark 🇩🇰).

When I'm not solving problems at work, I take loving care of my little family consisting of my wife and two <noscript>little</noscript> daughters <span id="daughters-ages"></span> 👨‍👩‍👧.

In weekends, I love playing mind-bending cooperative mystery games with friends or exercise and play with my daughters and many nephews.

I also enjoy learning new software development languages, techniques, patterns, and the like, when time permits. I'm writing about what I learn as an exercise for myself and as a lookup tool to see what I've learnt in the past. If anyone else finds this useful, I'm happy 🤓.

To be on the safe side: anything I write on this blog is my opinion, and mine alone, and not any of my employer.

<script>
    function getAge(dateString) {
        // thanks naveen: https://stackoverflow.com/a/7091965/618441
        var today = new Date();
        var birthDate = new Date(dateString);
        var age = today.getFullYear() - birthDate.getFullYear();
        var m = today.getMonth() - birthDate.getMonth();
        if (m < 0 || (m === 0 && today.getDate() < birthDate.getDate())) {
            age--;
        }
        return age;
    }

    const myAge = getAge('1990-11');
    const eldestDaughterAge = getAge('2019-04');
    const youngestDaughterAge = getAge('2022-07');

    document.getElementById('my-age').innerHTML = myAge + ' year';
    document.getElementById('daughters-ages').innerHTML = `ages ${eldestDaughterAge} and ${youngestDaughterAge}`;
</script>
