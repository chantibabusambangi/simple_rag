**simple RAG system**

**workflow:**

1.input data from the user, like any data like pdf,docs,text files,web url anything

2.**data loader** also called data injection, from langchain_community.**document_loaders**

3.**chunking or splitting**: from langchain.text_splitter import RecursiveCharacterTextSplitter and chunksize,chunkoverlap are paremeters
why to chunk the data:
      1.llms have specific context window size, so not exceeding that.
      2.Small, focused chunks make vector search more precise.
these chunks are called docs/data.

4.**embedding**
    we turn each chunk(also called token) into a vector[1,-0.97,-.2,....]
    this stored in db (as a zip file) for doc, meta in zip(data['documents'], data['metadatas']):
    
5.**query** from user
  query = "What did the Lion King learn?"
  print(db.similarity_search(query)[0].page_content)
  #results = db.similarity_search(query, k=3) default k can be 4
  based on similarity_search we retrieve relevant context/chunks
  results = db.similarity_search(query, k=3) #getting 3 chunks
  
6.**retrival & chain**: chian takes query,which is passed to retriver to fetch retrival documnets
  and thses documents are input for llms.
  retriver: it is an interface that  takes string a input and return list of k chunks as o/p #same like similairity search.
  chain:a sequence of calls to llm and retriver.
        after getting k relevant chunks from retriver, it builds/plugs the prompts (actual query + k relevent chunks) to llm.
  retriver=db.as_retriever()
  #uses db as backbobe
  we combine both retreiver+chian to get desired response

**DOUBTS**
      1.to get k relevant chunks, we can get from similarity_search it self right, instead of retriver?
      
      sol:  yes!  chain doesn’t care how you retrieve — you can swap in. but
      
            1.It follows a standard interface.
            
            2.Hybrid retriever (e.g. keyword + vector)
            
            3.Building a reusable Chain 
            
            4.Best practice for pluggable, maintainable RAG
