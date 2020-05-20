오늘 여기서는 
```
import 'dart:io';

import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';
import 'package:image_picker/image_picker.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  List<File> _file = [];
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: SafeArea(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.stretch,
              children: <Widget>[
                Expanded(
                    flex:1,
                    child: FlatButton(
                      color: Colors.blueAccent,
                      child:Text('Take a Picture'),
                      onPressed: () async{
                        File file = await ImagePicker.pickImage(source: ImageSource.gallery);
                        setState(() {
                          _file.add(file);
                        });
                      },
                    )
                ),
                Expanded(
                    flex:10,
                    child: ListView.builder(
                      itemCount: _file.length,
                      itemBuilder: (context, count){
                        return Stack(
                          children: [
                            InkWell(
                                onTap: () {
                                  Navigator.push(context, MaterialPageRoute(
                                    builder: (BuildContext context) {
                                      return Container(
                                        child: Center(
                                          child: Image.file(_file[count]),
                                        )
                                      );
                                    },
                                  ));
                                },
                                child: Image.file(_file[count])
                            ),
                            Positioned(
                              right:1.0,
                              child: Container(
                                decoration: BoxDecoration(
                                  color: Colors.red,
                                  borderRadius: BorderRadius.circular(50),
                                ),
                                child: IconButton(
                                    icon: Icon(Icons.close, color: Colors.white, size:30),
                                    onPressed: (){
                                      setState(() {
                                        _file.removeAt(count);
                                      });
                                    },
                                ),
                              )
                            )
                          ]
                        );
                      },
                    ),
                ),
                Expanded(
                  flex:1,
                  child: FlatButton(
                    child: Text('전송하기'),
                    onPressed: () {
                      print(_file);
                    },
                  )
                )
              ],
            )
        ),
      ),
    );
  }
}
```
