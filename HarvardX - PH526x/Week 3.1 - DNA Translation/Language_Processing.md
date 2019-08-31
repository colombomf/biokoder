###  PH526x  | HarvardX 2019 Summer Class
## Using Python For Research 
### Language Processing



    text = 'So much has gone and little is new and as the sparrow sings dawn 
    chorus for someone else to hear the thinker sits alone growing older 
    and so bitter'
    
    def count_words(text):
        """
        Count the number of times each word occurs in text (str). 
        Return dictionary where keys are unique words and values are word counts.
        """
        word_counts = {}  
        for word in text.split(" "):
            if word in word_counts:  
                word_counts[word] += 1  
            else:  
                word_counts[word] = 1  
            return word_counts


,

    count_words(text)

##

    def count_words(text):
        """
        Count the number of times each word occurs in text (str). 
        Return dictionary where keys are unique words and values are word counts. 
        Skip punctuation.
        """
        text = text.lower()  
        skips = [".", ",", ";", ":", "'", '"']  
        for ch in skips:  
            text = text.replace(ch, " ")  
            word_counts = {}  
            for word in text.split(" "):  
			    #known word  
                if word in word_counts:  
                    word_counts[word] += 1  
			    #unknown word  
                else:  
                    word_counts[word] = 1  
            return word_counts

#

    from collections import Counter
    def count_words_fast(text):
    	"""
        Count the number of times each word occurs in text (str). 
        Return dictionary where keys are unique words and values are word counts. 
        Skip punctuation.
        """
    	text = text.lower()
    	skips = [".", ",", ";", ":", "'", '"']
    	for ch in skips:
    		text = text.replace(ch, " ")
    
    	word_counts = Counter(text.split(" "))
    	return word_counts
.

    count_words_fast(text) == count_words(text)
#

    def read_book(title_path):
        """
        Read a book and return it as a string.
        """  
        with open(title_path, "r", encoding = "utf-8") as current_file:  
            text = current_file.read()  
            text.replace("\n", "").replace("\r", "")  
        return text
.



    len(text)

    ind = text.find("sometextfromfile")  

	ind

    sample_text = text[ind : ind + 1000]
#

       def word_stats(word_counts):
       """
       Return number of unique words and word frequencies.
       """      
       	num_unique = len(word_counts)  
       	counts = word_counts.values()  
        return (num_unique, counts)


.
	  
    word_counts = count_words(text)
    
    num_unique

    sum(counts)

##
### Note:
Questionnaires may not be the same from year to year but are conceptually similar.
Please read the statement carefully before submission.


