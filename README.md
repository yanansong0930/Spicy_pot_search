# Spicy_pot_search
A search engine designed for UCI websites

server: the back-end files deployed on server. Including webserver and search algorithm

db_generator: generator scripts for creating index-posting database

crawler and web pages are not shown because of size.

# Work Flow
User Input Query

Lemmatize

Multiple Search by NLP Synonyms Extension

Tokenize

CASE:

	One Word Query:
	
	Go over the 1-word index to create fast rank
		Piority of ranking class
			Title, Anchor
			Title
			Anchor
			Normal
	Calculate Cosine Similarity of each ranking class
	First-Page 10 Links --- T,A:4; T:3; A:2; N:1;
	Rest Pages: Listing By rancking class and cosine score
	
	Two Word Query:
	
	If Found in 2-word index
		Go over the 2-word index to create fast rank
			Piority of ranking class
				Title, Anchor
				Title
				Anchor
				Normal
		Calculate Cosine Similarity of each ranking class
		First-Page 10 Links --- T,A:4; T:3; A:2; N:1;
		Rest Pages: Listing By rancking class and cosine score
	ELSE
		Same Algorithm with multi token search, no phrase indicated.
	
	
	More than 2-words Query:
	
	If Given Phrase:
		If phrase not found, execute algorithm below
		Go over the 2-word index to create fast rank
			Piority of ranking class
			Title, Anchor
			Title
			Anchor
			Normal
		Calculate Cosine Similarity of each ranking class, Phrase use 2-word index
		Top max(40,len(T,A+T+A)) Links will be enhenced with Position Searching
		Calculate Position Weight for top links, Formula: 先算各个单词出现位置的平均数，然后以方差排序
		Top links' Cosine Similarity will be replaced by position weight
		First-Page 10 Links --- T,A:4; T:3; A:2; N:1;
		Rest Pages: Listing By rancking class and cosine score/position score
	
	If not given Pharase:
		If longest 2-gram found in 2-word index, then execute algorithm above
		Go over the 1-word index to create fast rank
			Piority of ranking class
			Title, Anchor
			Title
			Anchor
			Normal
		Calculate Cosine Similarity of each ranking class, use 1-word index
		Top max(40,len(T,A+T+A)) Links will be enhenced with Position Searching
		Calculate Position Weight for top links, Formula: 先算各个单词出现位置的平均数，然后以方差排序
		Top links' Cosine Similarity will be replaced by position weight
		First-Page 10 Links --- T,A:4; T:3; A:2; N:1;
		Rest Pages: Listing By rancking class and cosine score/position score

	Return Empty or Results List to Front End
	Send to GUI
