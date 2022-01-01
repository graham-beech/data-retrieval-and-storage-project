Contact info: graham.scott.beech@gmail.com (graham) and erol1@stolaf.edu (besna)

{User Manual: Data Retrieval & Storage Project}

~ Program Description ~
  The purpose of this program is to locate, retrieve, and store specific HTML data from many pages of a website when provided with only one url. The location of HTML data within a website is specific to only that website. For this reason our program only functions on one site: http://books.toscrape.com, a website with liberal robots.txt policies made specifically for web scraping projects. Yet much of this code is transferable to other sites, provided that relatively small adjustments are made.
  To automate the process of scraping data from many pages of the given website, this program receives a seed url. Using this seed-url as a starting point, the program derives many branch-urls recursively through an accumulator pattern with an if-else base case. A method from the Request Module is used to retrieve HTML data from each branch-url in turn. Then the dense HTML data retrieved from the branch-url in question is parsed by Python's HTML parser and organized into a parse tree by the BeautifulSoup Module. This program calls the methods of BeautifulSoup objects (which correspond with each derived branch-url) to retrieve relevant data; and, this data is then written to a .csv file for hypothetical analysis in Excel, SPSS, or R. 

~ System Requirements ~
This program was built in Python 3.10.1 
It requires the following modules: BeautifulSoup (v.4), CSV, Requests, and Time. 
These modules may require installation through pip or your OS’ Command Line.
For assistance please contact graham.scott.beech@gmail.com

~ Instructions ~
	After installing the necessary modules (see System Requirements), simply navigate to the “Run” button of your Python Editor (IDLE was used to write and test the program but any editor should work). Click Run→Run Module, or your Python Editor’s equivalent of the Run Module button (which executes the code). Please wait for the program to run– you can observe the progress in your Python Shell if you would like to. When your Shell reads “Data collection completed.” you may navigate to the folder directory in which you have stored the program to observe that a new .csv file titled “book_data.csv” has been created. If Excel is your default program for opening .csv files, then the file may appear alongside a green Excel icon. Feel free to open this file with Excel, or another program to which you are inclined, and observe that 1000 rows of data have been written to the file. You may try analyzing the data if you would like to but this particular website contains dummy data, so the books’ price- and rating-data is arbitrary. Yet please note that were this data to be real then pairing this scraped data with analytical software could most certainly yield insight and clarity. The great utility of data-driven analysis across fields implies that the prerequisite data collection is worthwhile.
	Should the comments added to the program not be enough to clarify the inner working of this project, then I would want to communicate a few things. In a nutshell the program creates several urls from one seed (or parent) url, then each of these urls is searched for programmer-specified data (here, specifically book titles, ratings, prices, and availability for the books on that page), and finally that data is written to a file. The whole process of “url creation → data scraping → data writing” begins and completes in totality for one branch url (or child url) before the program begins the process for the next branch/child url.

~ Technical Guide ~
	If more detail is needed than is provided in the insturctions above and the comments of the program itself, then I would invite you to read the rest of this paragraph and the next two. This program is using two classes, ‘Page_Scraper’, and ‘Subpage_Deriver’. What’s going on with these two is that an object of class Subpage_Deriver is created such that its __init__ takes a seed url; and, this created object has its method “create_subpage” invoked. This moment is crucial because it is this create_subpage method that will be recursively invoked. The create_subpage method then generates a new url using the previously mentioned seed url and an accumulator set to 1 on the first invocation of create_subpage. Create_subpage invokes another method of class Subpage_Deriver called create_soup, which does a number of things. First it retrieves the html of the specified url via the Request module. Next it parses through the dense string version of the html text with html.parser, and creates a parse tree with BeautifulSoup, assigning the tree the variable name soup_A. Lastly, the BeautifulSoup method of soup_A called “find” is used to determine whether there is a next button on the current webpage. 
  If there is such a button then an object of class Page_Scraper is created. Its __init__ method receives soup_A as its argument, which is assigned to self.soup, which itself is then used with BeautifulSoup method find_all to locate all “books” (where “books'' is really a str with html formatting) on this particular page. This data corresponding with books is given the name self.books. Next, within the create_soup function the retrieve_bookpod_data method of Page_Scraper class is invoked. This invoked method goes through each book in turn, retrieves the title, rating, price, and availability of that book by invoking BeautifulSoup methods, adds these data points to a list, and invokes the write_data method to write this list (of book data) to a new row of a .csv file. After all the requested data from every book from the Soup object which corresponds with one subpage is retrieved and written, then within the create_soup method the program uses the time module to rest for two seconds. This prevents harm done to the webpage and server hosting the webpage. Next the previously mentioned accumulator variable (page_num) is increased by one, and create_subpage is called recursively. This time the url will change to reflect the change in the accumulator variable, and similarly the soup, the books within the soup, and the data within the books within the soup will be different also— different both when retrieved and when written to the .csv file, in that the data reflects the change of “page” of the website by one. This loop of arriving at the accumulator, raising it by one and running through create_subpage and create_soup again will continue until the base case, (conditioned by the “else”) is hit. 
  This brings us back to what happens when the BeautifulSoup method of soup_A called “find” is used to determine whether there is a next button on the current webpage-soup, and there is no such a button. When there is no such a button then, per the indented commands following the “else:”-statement, an object named q of the class Page_Scraper is created, and the __init__ receives the argument soup_A (which is the soup constructed from the most recent url) as its parameter. Within the else statement, the retrieve_bookpod_data method is invoked, and the previously detailed commands of this method are executed. Lastly the Shell prints “Data Collection Completed”, and the initial invocation of create_subpage from main received the return value True. At this point then program has finished its job. You can now open your newly written .csv file, and analyze the 1000 rows of data in your favorite analytical program!
