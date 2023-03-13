submission.csvをダウンロード

```python
submission.to_csv('submission.csv',index=False)

from google.colab import files
files.download('submission.csv')
```
