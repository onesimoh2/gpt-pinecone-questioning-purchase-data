
# Use GPT to find embedding vectors and Pinecone to store them. Then # using both to answer queries about purchase data.

## Adding data into a Pinecone index.

Embeddings for the purchase data were already calculated in the repo 'gpt-embedding-sales-data-for-searching' and stored in 'sales_data_emb.csv'.

In the notebook called 'CreateIndex' this csv is loaded and used create pairs of id and embeddings for the Pinecone upload functions. We are not adding explanation of how this is done because it follows the standard Pinecone procedures that can be read in the Pinecone documentation.

## Using Pinecone to support the retrieval of purchase information

The process implemented in the notebook 'QueryTheIndex' is similar to the one developed in the notebook ‘QueringData’ in repo 'gpt-embedding-sales-data-for-searching' with the following simplifications: first no parsing of the query to detect queries outside the scope of the app and second no cache data is added to the query.

The main difference with the 'gpt-embedding-sales-data-for-searching' is that here we are not assumng that if GPT-4 cannot extract to wich dates the query is refering to then, instead of assumning the last 6 months, Pinecone is queried. After converting the query to an ebedded vector it is passed to a Pinecone query and it will return the dates that are more related to the query and those will be used the add the respective data to the final request to GPT-4.

## Example

The system is asked the following question:

### Tell me all the dates when I bought office related products
As you can see there is no way that any specific dates can be extracted from here. But the query to Pinecone did produce similar vectors corresponding to certain dates and those dates are used to ger the proper data from the database and injected into the query. The response is:

Here are the dates when you bought office-related products:

1. January 26, 2016:
   - Developer joke mug - that's a hardware problem (White)
   - Office cube periscope (Black)
   - Packing knife with metal insert blade (Yellow) 9mm
   - Shipping carton (Brown) 500x310x310mm

2. April 30, 2015:
   - Developer joke mug - Oct 31 = Dec 25 (White)
   - Developer joke mug - that's a hardware problem (White)

3. July 7, 2015:
   - Air cushion film 200mmx200mm 325m
   - DBA joke mug - you might be a DBA if (White)

   The list continue ...

   As you can see it did a pretty god job of selecting the dates when possible products related to an office were purchased.

   ## Conclusion

    Pinecone is a powerful vector database with the necessary features for document searching. It is not too good though for factual data like the one we handle here. But it was used cleverly to support the algorithm when no interval of dates in possible to infere from the query.




