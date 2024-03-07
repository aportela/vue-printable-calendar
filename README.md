# vue-printable-calendar

Generate a calendar for the current month/year to be printed on an A4 size paper with horizontal orientation.

# Why

Every month, I need to print out a calendar on paper, so I asked ChatGPT (3.5) to help me and thus begin to evaluate its potential collaborative use in programming tasks. Despite making several mistakes and having to adjust the prompts successively, as well as reviewing and testing all the code, I have been pleasantly surprised.

## Showcase

## TLDR (I don't want to complicate my life, just print a calendar)

Download / unzip last build release in a path of your web server and point your browser to the corresponding URL.

## Development

### Install

```
$ git clone https://github.com/aportela/vue-printable-calendar.git

$ cd vue-printable-calendar

$ npm install
```

### Build

```
$ npm run build
```

## Props

**locale**: String (optional)

    Description: locale used for translating weekdays/month names

**startAtSunday**: Boolean (optional)

    Description: week starts at sunday / monday

**fullNames**: Boolean (optional)

    Description: use long/short names on weekday/month

**month**: Number

    Description: default month (1-12)

**year**: Number

    Description: default year
