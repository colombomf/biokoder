###  PH526x  | HarvardX 2019 Summer Class
## Using Python For Research 
### Plotting Statistics



    import os 
    book_dir = ("./file_path_here")
    
    for language in os.listdir(book_dir):
       for author in os.listdir(book_dir + "/" + language): 
           for title in os.listdir(book_dir + "/" + language + "/" + author):
               inputfile = book_dir + "/" + language + "/" + author + "/" + title
               print(inputfile)
               text = read_book(inputfile)
               (num_unique, counts) = word_stats(count_words(text))


##



    import os
    
    book_dir = ("./file_path_here")
    import pandas as pd
    stats = pd.DataFrame(columns = ("language", "author", "title", "length", "unique"))
    title_num = 1
    for language in os.listdir(book_dir):
        for author in os.listdir(book_dir + "/" + language):
                for title in os.listdir(book_dir + "/" + language + "/" + author):
                    inputfile = book_dir + "/" + language + "/" + author + "/" + title
                    print(inputfile)
                    text = read_book(inputfile)
                    (num_unique, counts) = word_stats(count_words(text))
                    stats.loc[title_num] = language, author, title, sum(counts), num_unique
                    title_num += 1
    
#

        
    import os
    book_dir = ("./file_path_here")
    import pandas as pd
    stats = pd.DataFrame(columns = ("language", "author", "title", "length", "unique"))
    title_num = 1
    for language in os.listdir(book_dir):
       for author in os.listdir(book_dir + "/" + language): 
           for title in os.listdir(book_dir + "/" + language + "/" + author):
               inputfile = book_dir + "/" + language + "/" + author + "/" + title
               print(inputfile)
               text = read_book(inputfile)
               (num_unique, counts) = word_stats(count_words(text))
               stats.loc[title_num] = language, author.capitalize(), title.replace(",txt", ""), sum(counts), num_unique
               title_num += 1

#

    stats.length

    stats.unique

---

    import matplotlib.pyplot as plt  
    plt.plot(stats.length, stats.unique, "bo") #blue orbs

---

    import matplotlib.pyplot as plt  
    plt.loglog(stats.length, stats.unique, "bo")

---

    stats[stats.language == "English"]

#


    plt.figure(figsize = (10, 10))  
    subset = stats[stats.language == "English"]  
    plt.loglog(subset.length, subset.unique, "o", label = "English", color = "crimson")
    
    plt.figure(figsize = (10, 10))  
    subset = stats[stats.language == "Portuguese"]  
    plt.loglog(subset.length, subset.unique, "o", label = "English", color = "orange")  
      
    plt.legend()  
    plt.xlabel("Book length")  
    plt.ylabel("Number of unique words")  
    plt.savefig("lang_plot.pdf")

##
### Note:
Questionnaires may not be the same from year to year but are conceptually similar.
Please read the statement carefully before submission.




