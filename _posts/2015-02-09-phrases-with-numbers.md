---
layout: post
title: "Displaying phrases containing numbers"
description: "Formatting phrases containing numbers in Swift"
date: 2015-02-09 02:00:00
category: programming
tags: 
    - swift
---

I'm in the middle of writing an app in Swift which requires an alert to appear which says something like "10 questions entered".  But of course some times it should say "1 question entered".  The traditional approach to the question/questions situation is to either test for the number and create a string accordingly or to use the rather tacky "question(s). As I needed similar phrases in a number of places, I decided that the best approach was to create a function.  Although the app I'm writing is for OSX, the functions described below should work in iOS as well.

The functions can be downloaded [here](https://gist.github.com/jpopham/55ed82e095220d9a3c0b#file-gistfile1-txt)

### A generic format
In outline I wanted the function to be of the form:

    func countMessage(count:Int,message:String) ->String {...}
    
To meet a variety of possible cases I took the general case for message to be :
"some-or-no-text-[token]-some-or-no-text" repeated an arbitrary number of times, where [token] is in one of 

    [count] or [singularword:pluralword]
    
The usage is be something like:

    let amount = 1
    let input = "There [is:are] [count] [question:questions] outstanding"
    let output = countMessage(amount,input)
    println(output)
    
which would produce the output:

    There is 1 question outstanding
    
changing the value of amount to 2 produces the output:

   There are two questions outstanding

Although the function doesn't preclude [count] occurring more than once, it's hard to think of an example where it (sensibly) would.

### The function
Here is the function in its original form:
	{% highlight swift linenos %}
	func countMessage(count:Int,message:String) ->String {
		var outputMessage = ""
		var newArray:[String] = []
		var bits = split(message,{$0 == "["},allowEmptySlices:false)
		for part in bits {
			var bits2 = split(part,{$0 == "]"}, allowEmptySlices:false)
			for var i = 0; i < bits2.count; ++i {
				if bits2[i] == "count" {
						bits2[i] = "\(count)"
				}
				var bits3 = split(bits2[i],{$0 == ":"},allowEmptySlices:false)
				var token:String
				if bits3.count == 1 {
					token = bits3[0]
				} else {
					if count == 1 {
						token = bits3[0]
					} else {
						token = bits3[1]
					}
				}
				newArray.append(token)
			}
		}
		outputMessage = newArray.reduce("", +)
		return outputMessage
	}
	{% endhighlight %}

Line 4 creates a String array containing the input string 'message' split at each occurrence of "[".  With input of

	"There [is:are] [count] [question:questions] outstanding"

bits will contain:

    ["There", "is:are] ", " count] ", "question:questions] outstanding"]
    
Line 5 iterates through the bits array

Line 6 splits each element of the bits array at each occurrence of "]" and puts it in the bits2 array.  To continue the last example, for the last element of the bits array bits2 becomes

	["question:questions", " outstanding"]
	
Note that at this point all of the "[" and "]" will have been removed.

Line 7 iterates through the bits2 array

Lines 8, 9 and 10 replace the [count] token (which by now has become just "count") with the value of the `count` variable

Line 11 splits each element of the bits2 array at each occurrence of ":" and puts it in the bits3 array.  For example

	"question:questions"
	
becomes

	["question":"questions"]
	
Lines 12 to 21 deal with selecting the appropriate part of singular/plural pairs

Line 22 builds a new array comprising all the individual elements

Line 25 creates a string consisting of all the elements concatenated together, which is returned by line 26.

### Putting it in to words
Part way through creating `countMessage()` it occurred to me that there would be occasions when outputting the numeric value in words would be desirable.  There is a partial solution using NSNumberFormatter, which I have wrapped in a function intToWords() and enhanced to allow for the capitalisation of the first letter (if desired), which proved easy and to allow for differences in the conventional way that numbers are put into words in different countries .  For example in the US 153 is rendered as "one hundred fifty-three" (this is what NSFormatter provides), whereas in the UK the same number becomes "one hundred and fifty-three".  This proved much more difficult.  Finally I put in an option to replace zero with some other word.

### The function
Here is the function

	{% highlight swift linenos %}
	func intToWords(integer:Int,capitaliseFirst:Bool = false,insertAnds:Bool = true,theWordForZero:String = "zero")->String{
		let formatter = NSNumberFormatter()
		formatter.numberStyle = NSNumberFormatterStyle.SpellOutStyle
		var outString = formatter.stringFromNumber(integer)!
		if insertAnds {
			var myDictionary = [
				"hundred o":"hundred and o",
				"hundred tw":"hundred and tw",
				"hundred thr":"hundred and thr",
				"hundred f":"hundred and f",
				"hundred s":"hundred and s",
				"hundred e":"hundred and e",
				"hundred n":"hundred and n",
				"huntdred te":"hundred and te",
				"hundred thi":"hundred and thi"
			]
			for (originalWord, newWord) in myDictionary {
				outString = outString.stringByReplacingOccurrencesOfString(originalWord, withString:newWord, options: NSStringCompareOptions.LiteralSearch, range: nil)
			}
			var zz:[String] = split(outString,{$0 == " "},allowEmptySlices:false)
			if zz.count>3 {
				if zz[zz.count-2] == "thousand" || zz[zz.count-2].hasSuffix("ion"){
					zz[zz.count-2] += " and"
				}
			}
			outString = zz.reduce("", {$0 + " " + $1})
			let subStart = advance(outString.startIndex, 1, outString.endIndex)
			outString = outString.substringWithRange(Range(start: subStart, end: outString.endIndex))
		}
		if outString == "zero" {
			outString = theWordForZero
		}
		if capitaliseFirst {
			outString.replaceRange(outString.startIndex...outString.startIndex, with: String(outString[outString.startIndex]).capitalizedString)
		}
		return outString
	}
	{% endhighlight %}

Lines 2 to 4 carry out the basic number to string conversion (e.g. 153 becomes "one hundred fifty-three")

Line 5 checks whether we want "ands"s inserted in appropriate places.

Lines 6 to 16 sets up a dictionary of the trickiest before and after cases where we want to insert "and" after "hundred".  For example we do want to turn 153 into "one hundred and fifty-three", but we don't want to turn 100000 into "one hundred and thousand".

Lines 17 to 19 apply the dictionary to insert the necessary "and"s

Line 20 puts each word into a separate array element.

Lines 21 to 25 checks to see whether the second to last word is "thousand", "million", "billion" or any other word ending "ion" and if so adds " and" to it.

Line 26 joins the array back into a string, putting a space between each word.

For reasons I don't fully understand the resultant string has a spurious space at the start.  Lines 27 aand 28 remove this.

Lines 30 to 32 deal with replacing zero with another word.

Lines 33 to 35 capitalise the first letter if required.

### Making use of the intToWords() in compareMessage()
To make use of the intToWords function in compareMessage() a few small changes are needed to the latter.  Firstly, replace the first line with:

	{% highlight swift %}
	func countMessage(count:Int,message:String,numberAsWords:Bool = false, wordForZero:String="zero",capitaliseCountWordAlways:Bool = false, capitaliseCountWordIfFirst:Bool = true, insertAnds:Bool = false)->String{
	{% endhighlight %}
	
Secondly replace line 5 with this:

	{% highlight swift %}
	for (j,part) in enumerate(bits) {
	{% endhighlight %}

This uses the variable j to keep track of which element of the array bits is under scrutiny

Lastly replace line 9 with this:

	{% highlight swift %}
	if numberAsWords {
		if j==0 || capitaliseCountWordAlways{
			bits2[i] = intToWords(count, capitaliseFirst: capitaliseCountWordIfFirst,insertAnds:insertAnds,theWordForZero:wordForZero)
		} else {
			bits2[i] = intToWords(count, capitaliseFirst: capitaliseCountWordAlways,insertAnds:insertAnds,theWordForZero:wordForZero)
		}
		
	} else {
		bits2[i] = "\(count)"
	}
	{% endhighlight %}
	
This uses j to determine whether we are at the beginning of the string, checks whether we want to output count in words and calls intToWords with appropriate parameters if required.  Note that the parameters of the revised function allow for capitalisation of the first letter of the stringified(!?) count to always be capitalised or only if the [count] token appears at the beginning of the message string.

The functions can be downloaded [here](https://gist.github.com/jpopham/55ed82e095220d9a3c0b#file-gistfile1-txt)


