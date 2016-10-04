## Quiz

Small quizzes on languages we like to keep us in shape.

Select your language


- [English](en/README.md)


#### Example

- From `bash/samples/1.txt` print *second names*

Expected result

```txt
  Runolfsdottir
  Powlowski
  ....
  Walsh
```

<details>
  <summary>Answer</summary>

    awk '{ print $2 }' bash/samples/1.txt

</details>
