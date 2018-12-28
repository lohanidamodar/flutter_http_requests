# flutter_http

A Simple http request example with Future, json decode, async/await and FutureBuilder widget

## Packages used
- Dart Async (For working with Future objects using async/await)
- Dart Convert (For JSON parsing)
- http (For HTTP requests)

## Example Code

```dart
import 'package:flutter/material.dart';
import 'dart:async';
import 'dart:convert';
import 'package:http/http.dart' as http;

void main() async {

  runApp(
    MaterialApp(
      theme: ThemeData(
        primaryColor: Colors.red
      ),
      home: Scaffold(
        appBar: AppBar(title: Text("JSON"),centerTitle: true,),
        body: HomePage(),
      ),
    )
  );
}


class HomePage extends StatelessWidget {
  
  Future<List> getPosts() async {
    http.Response response = await http.get("https://jsonplaceholder.typicode.com/posts");
    return json.decode(response.body);
  }

  @override
  Widget build(BuildContext context) {
    return Container(
      child: FutureBuilder(
        future: getPosts(),
        builder: (BuildContext context, AsyncSnapshot<List<dynamic>> snapshot){
          if(snapshot.hasData){
            final List<dynamic> data = snapshot.data;
            return ListView.builder(
              itemCount: snapshot.data.length,
              itemBuilder: (BuildContext context, int index){
                final item = data[index];
                return Container(
                  padding: EdgeInsets.symmetric(horizontal: 10.0, vertical: 5.0),
                  child: ListTile(
                    title: Text(item["title"], style: TextStyle(
                      color: Theme.of(context).primaryColor,
                      fontSize: 20.0,
                      fontWeight: FontWeight.w600
                    )),
                    subtitle: Text(item["body"], style: TextStyle(
                      fontSize: 16.0
                    ),),
                  ),
                );
              },

            );
          }else{
            return Center(child: CircularProgressIndicator());
          }
        },
      ),
    );
  }
}


```