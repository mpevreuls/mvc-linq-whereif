# mvc-linq-whereif
Extenstion to the MVC Linq functionality to better/easier support WHERE IF conditions.

This piece of code is in my own utilityclass. It makes it easier to use IF conditions in the WHERE clause within LINQ expressions.


         /// <summary>
        /// Defines the _notOkCharacter_
        /// </summary>
        public static IEnumerable<TSource> WhereIf<TSource>(
            this IEnumerable<TSource> source, bool condition,
            Func<TSource, bool> predicate)
            {
              if (condition)
                  return source.Where(predicate);
              else
                  return source;
             }

        public static IQueryable<TSource> WhereIf<TSource>(
              this IQueryable<TSource> source, bool condition,
              Expression<Func<TSource, bool>> predicate)
              {
                  if (condition)
                      return source.Where(predicate);
                  else
                      return source;
              }
       
       In practice we use can use it as show below. WhereIf(condition, linq expression). So below we see that if the tblOrderrow.OD_CUSTOMER is not null, it creates a WHERE condition OD_CUSTOMER should be equal to tblOrderrow.OD_CUSTOMER. If the tblOrderow.OD_CUSTOMER is equal to NULL, this WHERE condition is being skipped.
       
       var companies = db.tblOrderrow
                                    .WhereIf(tblOrderrow.OD_CUSTOMER != null, x => x.OD_CUSTOMER == tblOrderrow.OD_CUSTOMER)
                                    .WhereIf(tblOrderrow.OD_SUPPLIER != null, x => x.OD_SUPPLIER == tblOrderrow.OD_SUPPLIER)
                                    .WhereIf(tblOrderrow.OD_STATUS != null, x => x.OD_STATUS == tblOrderrow.OD_STATUS)
                                    .WhereIf(tblOrderrow.OD_ORDERDATE != null, x => x.OD_ORDERDATE >= tblOrderrow.OD_ORDERDATE)
                                    .ToList().OrderByDescending(c => c.OD_ORDERDATE);
       
