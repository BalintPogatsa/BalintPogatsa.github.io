---
layout: post
title: Custom procedures in Entity Framework 6
date: 2016-09-26
---

Starting from Entity Framework 6.1 it's possible to call stored procedures not only with Model-First, but also if you use Code-First approach.

There is a 3rd party library called [CodeFirstStoreFunctions](https://codefirstfunctions.codeplex.com/ "CodeFirstStoreFunctions") created by Pawel Kadluczka ([nuget package](https://www.nuget.org/packages/EntityFramework.CodeFirstStoreFunctions "nuget package")) which makes it much easier.

In my situation I wanted to fine-tune a Linq to Entities query to improve the performance:

                measurement = context.Measurements
				   .Where(m => m.MeasuredAt == Functions.ToDateLocal("2016.01.01 10:00:00", "YYYY.MM.DD Hh24:MI:SS"))
				   .FirstOrDefault();

The ToDateLocal function is a function implemented in the database (in my case in Oracle XE).
The reason for using it is that my table has the Oracle DATE type, but Entity Framework converts the .NET DateTime type to TIMESTAMP, which means that the performance can be much worse.

With the CodeFirstStoreFunctions library I can extend the Linq to Entities query in the following way:
(In the database I've specified the TO_DATE_LOCAL function - for some reason if you use Oracle it only works if you specify the function names and the schema with capital letters)

        [DbFunction("CodeFirstDatabaseSchema", "TO_DATE_LOCAL")]
        public static DateTime ToDateLocal(string dateTimeToConvert, string convertFormat)
        {
            // no need to provide an implementation
            throw new NotSupportedException();
        }
		
Sometimes you just want to use some built-in procedures of your database which is not accessible directly from Linq. At the moment the CodeFirstStoreFunctions library doesn't support them, but I've added it using an extra attribute.
You can find the sources of this addition in [my fork](https://codefirstfunctions.codeplex.com/SourceControl/network/forks/BalintPogatsa/BuiltInFunctions "my fork").

The way how to use it:

        [DbFunction("CodeFirstDatabaseSchema", "TO_DATE")]
        [DbFunctionDetails(IsBuiltIn=BuiltInOptions.BuiltIn)]
        public static DateTime ToDate(string dateTimeToConvert, string convertFormat)
        {
            // no need to provide an implementation
            throw new NotSupportedException();
        }
		
You can download the sources of my example from [here](https://github.com/BalintPogatsa/BalintPogatsa.github.io/raw/master/attachments/CustomFunctionsInLinqQueries.7z "here")