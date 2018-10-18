---
layout: post
title: Markdown Study Note
category: Misc
tags: Misc
keywords: Markdown
published: true
---

# Markdown Study Note
---------------------------------------
***Note: code top, view down***

## Two spaces or multiple [Return] to create new line


## ---, ***, ___ for sperator line


## Use # for Header
<pre>
# Header-1
## Header-2
### Header-3
</pre>


## Raw code
```text
<pre>  ...  </pre>
```


## Citation / Reference

<pre>
> Outer reference

>> Inner reference  
inner continue  

> outer reference continue
</pre>

> Outer reference

>> Inner reference  
inner continue  

> outer reference continue


## List
<pre>
* list 1
	* list 1.1
		* at most 3 layers
			* what happend to the 4th layer
				* what happend to the 5th layer
* list 2
</pre>

* list 1
	* list 1.1
		* at most 3 layers
			* what happend to the 4th layer
				* what happend to the 5th layer
* list 2

<pre>
1. item 1
2. item 2
3. item 3
</pre>

1. item 1
2. item 2
3. item 3


## Code
<pre>
```
int main(int argc, const char * argv[])
{
    printf("Hello, World!\n");
    return 0;
}
```
</pre>

```
int main(int argc, const char * argv[])
{
    printf("Hello, World!\n");
    return 0;
}
```
<pre>
</pre>