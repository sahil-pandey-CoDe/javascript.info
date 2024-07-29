We generally use upper case for constants that are "hard-coded". In other words, when the value is known prior to execution and directly written into the code.

In this code, `birthday` is exactly like that. Thus we could use the upper case for it.

In run-time, `age` is evaluated. We have one age now, and we will have another one in a year. It remains unchanged as a result of the code's execution, making it constant. However, compared to `birthday`, it is "less of a constant" because it is calculated, thus we should leave it in lower case.