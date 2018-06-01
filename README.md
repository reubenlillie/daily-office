# Daily Office
_The Daily Office Lectionary from the Book of Common Prayer (BCP) in JSON_

There are many projects which utilize the Daily Office Lectionary, but to my knowledge none of them shares an open source API or publishes any underlying data files.

This project is different. If you can dream of it with JSON, then these files are here to help.

## File Structure
Each office is a separate object. Office objects, in turn, are organized into arrays.

To manage size and shape better, each array of offices is contained with in a separate file. Both multiline and minified versions are available for the following unnamed arrays.

* Year One
* Year Two
* Holy Days
* Special Occasions

## JSON Schema
Offices follow the same basic pattern. For each office, there is at least (a) one psalm for either Morning Prayer or Evening Prayer and (b) one lesson from the Hebrew Bible, Apocrypha, and/or New Testament for either a specific calendar date or a day of the week with a season.

The following schema represents this pattern while allowing for the particular shape any given office may take.

### Base schema for all offices

`{`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`"psalms": {`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`}`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`"lessons": {`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`}`<br/>
`}`<br/>

### All potential property keys and values for any given office

`{`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`"year": "Year One or Year Two",`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`"season": "Advent, Christmas, Epiphany, Lent, Easter, The Season after Pentecost",`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`"week": "Week of # Season, Ash Wednesday and Following, Holy Week, Easter Week, or Proper #",`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`"day": "Day of week (days are in "long" format, e.g., 'Sunday, Monday, Tuesday, etc.') or Calendar date (months are in "short" format, e.g., 'Dec 24, Jan 5, etc.')",`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"title": "The First Sunday of Advent, The Second Sunday in Lent, The Day of Pentecost: Whitsunday, etc.",`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`"psalms": {`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"morning": [`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"Chapter and verse range for a given psalm (e.g., "23", "110:1–5(6–7), [133], etc."—[optional psalm] or (suggested lengthening)) as they appear in BCP 2007"`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`],`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"evening": [`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"Chapter and verse range for a given psalm (e.g., "23", "110:1–5(6–7), [133], etc."—[optional psalm] or (suggested lengthening)) as they appear in BCP 2007"`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`]`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`},`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`"lessons": {`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"morning": { // Optional object if BCP 2007 specifies`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`},`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"evening": { // Optional object if BCP 2007 specifies`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`}`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"first": "Book abbreviation, chapter, and verse range typically from the Hebrew Bible or Apocrypha (e.g., Isa 1:1–9)",`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"altFirst": "optional replacement for first",`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"second": "Book abbreviation, chapter, and verse range typically from the New Testament",`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"altSecond": "optional replacement for second",`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"third": "Book abbreviation, chapter, and verse range typically from the New Testament",`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"gospel": "Book abbreviation, chapter, and verse range from either Matt, Mark, Luke, or John",`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"altGospel: "optional replacement for gospel or, rarely, a second alternate to the second reading"`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`},`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`"notes": [`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"Note from BCP 2007"`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`]`<br/>
`}`

NB, `psalms.morning` and `psalms.evening` are _arrays_, and at least one array appears in each office. But `lessons.morning` and `lessons.evening` are _objects_ for accessing the named properties `first`, `second`, `gospel`, and so forth—appearing only where all lessons for a given office are intended specifically for either Morning Prayer or Evening Prayer. Otherwise, `first`, `second`, `gospel`, and the like are properties of the `lessons` object.

## Formatting

In Year One and Year Two, offices are combined for days that have special evening readings in addition to their regular readings. For example, December 24 and Christmas Eve are separate offices in BCP 2007, but they are a single office object with both a `day` and `title` here.

Values for the `title` property are adapted from "The Titles of the Seasons, Sundays, and Major Holy Days" ([BCP 2007, 31](https://www.episcopalchurch.org/files/book_of_common_prayer.pdf#page=31)).

All Bible book abbreviations are adapted to conform to the [Society of Biblical Literature standard](https://www.sbl-site.org/).

Only `notes` that apply to specific offices and could not otherwise be interpretted within the schema are included. For example, the note for `Jan 3` and `Jan 4` which instructs `"If today is Saturday, use Psalms 23 and 27 at Evening Prayer."` But the alternate readings in Judith in place of Esther for  `Proper 18` and `Proper 19` in `Year Two` are treated as `altFirst` on those days. Also, notes that apply to several days have been omitted—like Psalm 95 is "For the Invitatory" on Fridays in Lent. That way, such notes can be handled more programmatically rather than duplicating the same data across multiple objects.

## "Concerning the Daily Office Lectionary"
<blockquote>

The Daily Office Lectionary is arranged in a two-year cycle. Year One begins on the First Sunday of Advent preceding odd-numbered years, and Year Two begins on the First Sunday of Advent preceding even-numbered years. (Thus, on the First Sunday of Advent, 1976, the Lectionary for Year One is begun.)

Three Readings are provided for each Sunday and weekday in each of the two years. Two of the Readings may be used in the morning and one in the evening; or, if the Office is read only once in the day, all three Readings may be used. When the Office is read twice in the day, it is suggested that the Gospel Reading be used in the evening in Year One, and in the morning in Year Two. If two Readings are desired at both Offices, the Old Testament Reading for the alternate year is used as the First Reading at Evening Prayer.

When more than one Reading is used at an Office, the first is always from the Old Testament (or the Apocrypha).

When a Major Feast interrupts the sequence of Readings, they may be re-ordered by lengthening, combining, or omitting some of them, to secure continuity or avoid repetition.

Any Reading may be lengthened at discretion. Suggested lengthenings are shown in parentheses.

In this Lectionary (except in the weeks from 4 Advent to 1 Epiphany,and Palm Sunday to 2 Easter), the Psalms are arranged in a seven-week pattern which recurs throughout the year, except for appropriate variations in Lent and Easter Season.

In the citation of the Psalms, those for the morning are given first, and then those for the evening. At the discretion of the officiant, however, any of the Psalms appointed for a given day may be used in the morning or in the evening. Likewise, Psalms appointed for any day may be used on any other day in the same week, except on major Holy Days.

Brackets and parentheses are used (brackets in the case of whole Psalms, parentheses in the case of verses) to indicate Psalms and verses of Psalms which may be omitted. In some instances, the entire portion of the Psalter assigned to a given Office has been bracketed, and alternative Psalmody provided. Those who desire to recite the Psalter in its entirety should, in each instance, use the bracketed Psalms rather than the alternatives.

Antiphons drawn from the Psalms themselves, or from the opening sentences given in the Offices, or from other passages of Scripture, may be used with the Psalms and biblical Canticles. The antiphons may be sung or said at the beginning and end of each Psalm or Canticle, or may be used as refrains after each verse or group of verses.

On Special Occasions, the officiant may select suitable Psalms and Readings. ([BCP 2007, 934–935](https://www.episcopalchurch.org/files/book_of_common_prayer.pdf#page=934))
</blockquote>

Information concerning the Daily Office as a service or as [Daily Devotions for Individuals and Families](https://www.episcopalchurch.org/files/book_of_common_prayer.pdf#page=136) as well as additional directions from the BCP 2007 may be found on pp [35–146](https://www.episcopalchurch.org/files/book_of_common_prayer.pdf#page=35).

## References
BCP (The Episcopal Church). 2007. _The Book of Common Prayer and Administration of the Sacraments and Other Rites and Ceremonies of the Church Together with The Psalter or Psalms of David According to the use of The Episcopal Church_. New York: Church Publishing. https://www.episcopalchurch.org/files/book_of_common_prayer.pdf

## Contributing

Please submit any corrections or other ideas for improving the project either by creating an [issue](https://help.github.com/articles/creating-an-issue/) or [pull request](https://help.github.com/articles/creating-a-pull-request/).

## License
&copy; 2018 by [Reuben L. Lillie](https://twitter.com/reubenlillie)

All code and documenation for this project is licensed under the MIT License. See the [LICENSE.md](https://github.com/reubenlillie/daily-office/blob/master/LICENSE) file for details.

The Book of Common Prayer is not and never has been under copyright. The Episcopal Church first ratified the BCP in 1789 and last certified it in 2007.
