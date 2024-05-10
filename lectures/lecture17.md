<style TYPE="text/css">
code.has-jax {font: inherit; font-size: 100%; background: inherit; border: inherit;}
</style>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
    tex2jax: {
        inlineMath: [['$','$'], ['\\(','\\)']],
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'] // removed 'code' entry
    }
});
MathJax.Hub.Queue(function() {
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
});
</script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/MathJax.js?config=TeX-AMS_HTML-full"></script>


# 17. TreeMap and TreeSet

## TreeSet

```Java
TreeSet<Integer> schedule = new TreeSet<Integer>();
schedule.add(1223);
schedule.add(1430);
schedule.add(1545);
schedule.add(1610);
schedule.add(1705);
schedule.add(2010);
schedule.add(2215);
schedule.add(2320);
schedule.add(2345);

// show the last bus that leaves PGH before 4pm (strictly less than)
System.out.println(schedule.headSet(1600).last());

// show the first bus that leaves PGH after 10pm (greater than or equal to)
System.out.println(schedule.tailSet(2200).first());
```

## TreeMap

```Java
Map<String, Integer> freqOfWords = new HashMap<String, Integer>();
String[] words = "coming together is a beginning keeping together is progress working together is success"
        .split(" ");

for (String word : words) {
    Integer frequency = freqOfWords.get(word);
    if (frequency == null) {
        frequency = 1;
    } else {
        frequency++;
    }
    freqOfWords.put(word, frequency);
}

// no specific order in HashMap
System.out.println("Elements in HashMap: " + freqOfWords);

// print freqOfWords in ascending order
TreeMap<String, Integer> sortedWords = new TreeMap<String, Integer>(freqOfWords);
```


---

[Back to Home](../index.html)

