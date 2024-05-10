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


# 9. Comparable/Comparator

## Comparable
- Default way to compare is implement `Comparable` interface and override `compareTo()` method:
    - `compareTo()` method defines the **NATURAL (default)** orders of the class
    - `compare()` method defines different **CUSTOM (alternative)** orders of the class

- The class's `compareTo()` should be consistent to `equals()` and `hashCode()` such that:
    - If `x.equals(y)` is true, then `x.compareTo(y)` must equal to 0
    - If `x.equals(y)` is true, then `x.hashCode()` must equal to `y.hashCode()`


```Java
public class Card implements Comparable<Card> {
    private String suit;
    private int rank;

    /**
     * Constructs a card with suit and rank.
     * @param suit suit of a card
     * @param rank rank of a card
     */
    public Card(String s, int r) {
        suit = s;
        rank = r;
    }

    /**
     * Returns suit of a card.
     * @return suit
     */
    public String getSuit() {
        return suit;
    }

    /**
     * Returns rank of a card.
     * @return rank
     */
    public int getRank() {
        return rank;
    }

    /**
     * Returns String representation of card objects.
     * @return String representation of card objects
     */
    @Override
    public String toString() {
        return suit + ", " + rank;
    }

    /**
     * Uses rank to compare cards
     * @return positive, 0 or negative
     */
    @Override
    public int compareTo(Card other) {
        return Integer.compare(rank, other.rank);
    }


    /******************************************************************************
     *
     * Example of compareTo, equals and hashCode methods using Apache Commons Lang
     *
     ******************************************************************************/

    @Override
    public int compareTo(Card other) {
        return new CompareToBuilder()
                .append(this.suit, other.suit)
                .append(this.rank, other.rank)
                .toComparison();
    }

    @Override
    public boolean equals(Object other) {
        if (!(other instanceof Card)) {
            return false;
        }
        Card castOther = (Card) other;
        return new EqualsBuilder()
                .append(this.suit, castOther.suit)
                .append(this.rank, castOther.rank)
                .isEquals();
    }

    @Override
    public int hashCode() {
        return new HashCodeBuilder().append(this.suit).append(this.rank).toHashCode();
    }
    
    /************************************************************************
     *
     *  Example of compareTo, equals and hashCode methods using Google Guava
     *
     *  Note: When using Java 7 or later version, you can use Objects class
     *  in java.util instead of using Guava
     *
     ************************************************************************/

    @Override
    public int compareTo(Card other) {
        return ComparisonChain.start()
                .compare(this.suit, other.suit)
                .compare(this.rank, other.rank)
                .result();
    }

    @Override
    public boolean equals(Object obj){
        if(!(obj instanceof Card)){
            return false;
        }
        Card other = (Card) obj;
        return Objects.equal(this.suit, other.suit)
                && Objects.equal(this.rank, other.rank);
    }

    @Override
    public int hashCode(){
        return Objects.hashCode(this.suit, this.rank);
    }
    
}
```


## Comparator

```Java
public class CompareBySuit implements Comparator<Card> {
    /**
     * Implementation of compare method by suit.
     * @return positive, 0 or negative
     */
    @Override
    public int compare(Card x, Card y) {
        return x.getSuit().compareToIgnoreCase(y.getSuit());
    }
}
```


```Java
public class CompareBySuitRank implements Comparator<Card> {
    /**
     * Implementation of compare method using both suit and rank.
     * @return positive, 0, or negative
     */
    @Override
    public int compare(Card x, Card y) {
        int suitResult = x.getSuit().compareToIgnoreCase(y.getSuit());
        if (suitResult != 0) {
            return suitResult;
        }
        return Integer.compare(x.getRank(), y.getRank());
    }

}
```

## Collection.sort
```Java
// Default: Use compareTo() method implemented in Comparable class
Collections.sort(cards);

// Use compare() method implemented in Comparator class
Collections.sort(cards, new CompareBySuit());
Collections.sort(cards, new CompareBySuitRank());
```



---

[Back to Home](../index.html)