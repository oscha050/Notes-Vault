## Structure
### Headings
Use # up to 6 times for heading levels, the more the smaller
### Paragraphs 
Use <"p"> in the beginning of a paragraph and <"/p"> in the end of a paragraph to automatically format it.
Avoid using TAB or spaces in the beginning of a line as it can cause formatting errors
### Line Breaks
Add a line break at any point by using <"br"> (only in HTML can be used in md but a simple return works as well)
## Emphasis
### Bold
**Example text using bold**
Use ** or __ in both ends in markdown for bold text, ** works even in the middle of words, in HTML use <"strong"> in the beginning and <"/strong"> in the end instead
### Italics
*Example text using italics*
Use * or _ in both ends in markdown for italic text, * works even in the middle of words, in HTML use <"em"> in the beginning and <"/em"> in the end instead
### Bold and Italics
***Example text using bold italics***
Use ***  or any combination of the Bold/Italics alternatives in both ends in markdown for bold and italic text, *** works even in the middle of words, in HTML use <"em"><"strong"> in the beginning and <"/strong"><"/em"> in the end instead
### Blockquotes
>This is an example of a block quote
>>And this is a nested block quote

Use > for blockqoutes, for multiple paragraph blockquotes include a > on an empty line. For nested blockqoutes use >>. In order to avoid problems use a blank line before and after the blockqoute.
Most other md formatting works in blockqoutes
## Lists
### Ordered list
1. Use numbers with a dot
2. The number doesn't matter
Use 1. for ordered lists. The number doesn't matter as md will auto format numbers in falling order. Using indents will make sub lists. In HTML use <"ol"> and <"/ol"> for the list and <"li"> and <"/li"> for each item
### Unordered lists
- This is a unordered list
- Avoid mixing characters 
	- Indenting will give sub lists
- Using "-" will avoid software correcting it to **Bold** or *italics*
Use - or * or + for a unordered list. Do not mix between the options as this can cause problems. Best practice is using - as this will avoid potential conflicts with **bold** and *italics*. Indenting will give sub lists.
### Horizontal Rules

___

Use *** or --- or ___ by themselves on a line to create a horizontal line as shown above
### Links
Use [ ] for the name and follow with ( ) for the URL. With the link you can add text in " " to display when the link is hovered over with mouse. For a quick link enclose the URL in < >, the displayed text will be the URL. Also works with e-mail addresses. Emphasis works with links.
Alternatively putting the name in [ ] followed by a number in [ ] it will point to the number which will be formatted as: ["1"]: <"URL"> "Display when hovered on"
## Images
Use ! to add an image, using [text] you can add a image text followed by (link or Path to image) with optional " " by the link for same effect as in "Links"




## Tables
| HeadRow | column2 | Column3 |
| ------- | ------- | ------- |
| Row2    |         |         |
| Row3    |         |         |
Create a table by using | as spaces between the columns and the second row has to be on the form |-------|------|-------|. 