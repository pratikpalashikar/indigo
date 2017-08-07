---
title: "DataStructure #101 HashMap"
layout: post
date: 2017-08-05 
image: /assets/images/markdown.jpg
headerImage: false
tag:
- HashMap basics
- what is HashMap
- HashMap in different languages
- HashMap uses
- Space and time complexity of HashMap
- why use HashMap

category: blog
author: pratikpalashikar@gmail.com
description: Learn HashMap
# jemoji: '<img class="emoji" title=":ramen:" alt=":ramen:" src="https://assets.github.com/images/icons/emoji/unicode/1f35c.png" height="20" width="20" align="absmiddle">'
---


## HashMap Overview	:

   Hashmap is one of the most important data structure when it comes to accessing the data in the constant amount of time. Hashmap works in almost O(1) amount of time, which means it takes a constant amount of time to access the data. Now, many of you must be wondering what the heck he is talking about constant time and notation like "O(1)". Don't worry I am going to explain everything in detail.


###	HashMap Real Life Explanation : 
  	
   Many of you have read a book, have you ever struggled with finding a page which you were reading once you close a book. Yes ! what a solution to this problem? How can you go to the page you were reading directly? Many of you already have an answer in your mind, yes bookmark is the object that can help you find a page in a book easily. Great! you are one step closer to understanding how hashmap works.

   Let's try to analyse the real-life example and come up with the analogy between logical hashmap and the real-life situation. HashMap consists of the Key and Value pair. Key - unique value and Value - can be anything (String, integer, a book, a Page, a Person).

   - Key relates to the bookmark that helps us find a page in constant time. 
   - Value relates to the page which we were reading.

   Now here the term constant time comes into the picture. Always think of in terms of large data set and try to come up with the better solution. Let's say you have a book equal to your height with many pages and that book won't fit in your hand. And you want to search the page, you were reading. What do you think how much time you will take? You start, you randomly pick one page, you realise no it's not the page you want to read, you try again and again and again. It takes some time to find the page you were reading. This "search time" goes on increasing if the book with pages goes on increasing. This is what is termed as time complexity in the algorithm. Time to do the particular operation. 

   Now try using a bookmark, what do you think how much time will be required to search a particular page, even if the book has "n" number of pages. Yes ! you got it, it will take constant time. No matter how big book you are reading you will find a page in "fraction of time" but that time is so negligible which is almost termed as "constant" in computer science.
  	
  	  	
### Demonstration of time difference :
  	
Run the below program, you will find the difference yourself.
	
Program with HashMap :
   	
{% highlight html %}
		
import java.util.HashMap;
import java.util.Scanner;
public class SearchUtil{

	static HashMap<String,Integer> hmap = new HashMap<>();
	
	//Constructor
	public SearchUtil(){
		
		hmap.put("a",1);
		hmap.put("b",2);
		hmap.put("c",3);
		hmap.put("d",4);
		hmap.put("e",5);
		hmap.put("f",6);
		hmap.put("g",7);
	
	}
	
	
	public static void main(String []args){
	
		Scanner sc = new Scanner(System.in);
		SearchUtil su = new SearchUtil();
		System.out.println("Please enter the page you want");
		String input = sc.next();
		long startTime = System.nanoTime();
		System.out.println("Here is the page number you want : "+hmap.get(input));
		long stopTime = System.nanoTime();
		long elapsedTime = stopTime - startTime;
		System.out.println("Total time taken to execute the program : "+elapsedTime);
	}


}

{% endhighlight %}
	
	
Program without HashMap :
	
{% highlight html %}

import java.util.Scanner;
import java.util.ArrayList;


public class SearchUtil{

	static ArrayList<String> list = new ArrayList<>();	
	
	//Constructor
	public SearchUtil(){
		
		list.add("a");
		list.add("b");
		list.add("c");
		list.add("d");
		list.add("e");
		list.add("f");
		list.add("g");
	
	}
	
	
	public static void main(String []args){
	
		Scanner sc = new Scanner(System.in);
		SearchUtil su = new SearchUtil();
		System.out.println("Please enter the page you want");
		String input = sc.next();
		long startTime = System.nanoTime();
		int i=1;
		for(String listInput:list){
			if(listInput.equals(input)){
				System.out.println("Here is the page number you want :"+ i);
			}
			i++;
		}
		long stopTime = System.nanoTime();
		long elapsedTime = stopTime - startTime;
		System.out.println("Total time taken to execute the program : "+elapsedTime);
	}


}

{% endhighlight %}
  	
###	How HashMap is implemented in Java:

HashMap takes an extra space which grow with number of input. So the space complexity of the hashmap tends to O(n). where n is the number of input.

Let look at how the HashMap can be implemented: We will look at the creation of the hashmap from our own perspective.

{% highlight html %}

class MapDemo<K, V> {

	K key;
	V value;

	public MapDemo(K key, V value) {

		this.key = key;
		this.value = value;
	}

}

{% endhighlight %}

The above chunk represents the user defined data structure for Map. It's more of the generic structure, containing the Key and the Value. The Key and the value can be any object.

{% highlight html %}

// Generate Hash
public int generateHash(K key) {
	return (key.hashCode()) % (capacity);
}

{% endhighlight %}

This code is generated Hash based on the key and brings it within the range of the array capacity. It's like you have n number of buckets and you put an object into the buckets each hash generated point you to the bucket. Hashcode function usually refers memory address. 

{% highlight html %}

import java.util.ArrayList;
import java.util.Collection;
import java.util.Collections;
import java.util.LinkedList;

class MapDemo<K, V> {

	K key;
	V value;

	public MapDemo(K key, V value) {

		this.key = key;
		this.value = value;
	}

}

public class BoilerHashMap<K, V> {

	int capacity;

	ArrayList<LinkedList<MapDemo<K, V>>> hmap = null;

	// define the capacity for the hashMap
	public BoilerHashMap(int capacity) {
		this.capacity = capacity;
		hmap = new ArrayList<>(Collections.nCopies(capacity, null));

	}

	// Generate Hash
	public int generateHash(K key) {
		return (key.hashCode()) % (capacity);
	}

	// Put
	public void put(K key, V value) {

		int hashedKey = generateHash(key);

		// Put the key
		// Check if the hashkey already contains the values if it contains the
		// value they try to access the linked List and append it with values
		if (!(hmap.isEmpty()) && hmap.get(hashedKey) != null) {

			// add the value to the existing linked List
			MapDemo<K, V> mapDemo = new MapDemo<K, V>(key, value);
			hmap.get(hashedKey).add(mapDemo);

		} else {
			// Create the object of the linked List and add it to the Arraylist
			LinkedList<MapDemo<K, V>> temp = new LinkedList<>();
			MapDemo<K, V> md = new MapDemo<K, V>(key, value);
			temp.add(md);
			hmap.add(hashedKey, temp);
		}

	}

	// Get the value from the hashmap
	public V get(K searchKey) {

		int hashedValue = generateHash(searchKey);
		LinkedList<MapDemo<K, V>> values = hmap.get(hashedValue);

		if (values != null) {
			for (MapDemo<K, V> maps : values) {

				if ((searchKey.equals(maps.key))) {
					return (V) maps.value;
				}

			}
		}
		return null;
	}

	public static void main(String[] args) {

		BoilerHashMap<Integer, String> hmap = new BoilerHashMap<>(10);

		hmap.put(12, "Hello");

		System.out.println(hmap.get(12));

		hmap.put(13, "Hey there");

		System.out.println(hmap.get(14));

	}

}

{% endhighlight %}


The put and the get method are implemented as shown above. The hashmap above is implemented using ArrayList of LinkedList. Give it a try. If any question or suggestion please comment below.
  	
	

## If you like the blog please follow me on Github, Linkedin, twitter at the links mentioned above.




