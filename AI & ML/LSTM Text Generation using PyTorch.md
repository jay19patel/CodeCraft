# LSTM Text Generation using PyTorch

## Step 1: Prepare Data

### Raw Data:
```
what is your name : Jay
what is capital of India : Delhi
ok
```

### Preprocessing:
- Lowercase text
- Remove punctuation
- Tokenize the text

```python
import re

data = [
    "what is your name : Jay",
    "what is capital of India : Delhi",
    "ok"
]

tokens = []
for line in data:
    line = line.lower()
    line = re.sub(r'[^a-zA-Z ]', '', line)
    tokens.extend(line.split())

print(tokens)
```
**Output:**
```
['what', 'is', 'your', 'name', 'jay', 'what', 'is', 'capital', 'of', 'india', 'delhi', 'ok']
```

### Create Vocabulary:
```python
vocab = sorted(set(tokens))
word2idx = {w: i for i, w in enumerate(vocab)}
idx2word = {i: w for w, i in word2idx.items()}

print("Vocabulary:", word2idx)
```
**Output:**
```
{'capital': 0, 'delhi': 1, 'india': 2, 'is': 3, 'jay': 4, 'name': 5, 'of': 6, 'ok': 7, 'what': 8, 'your': 9}
```

---

## Step 2: Tokenize Input/Output Pairs

```python
sequence_pairs = [
    ("what is your name", "jay"),
    ("what is capital of", "india")
]

data_pairs = []
for inp, out in sequence_pairs:
    inp_tokens = [word2idx[w] for w in inp.split()]
    out_token = word2idx[out]
    data_pairs.append((inp_tokens, out_token))

print("Tokenized Pairs:", data_pairs)
```
**Output:**
```
[([8, 3, 9, 5], 4), ([8, 3, 0, 6], 2)]
```

---

## Step 3: Create Dataset and DataLoader

```python
import torch
from torch.utils.data import Dataset, DataLoader

class TextDataset(Dataset):
    def __init__(self, data_pairs):
        self.data = data_pairs

    def __len__(self):
        return len(self.data)

    def __getitem__(self, idx):
        x, y = self.data[idx]
        return torch.tensor(x), torch.tensor(y)

dataset = TextDataset(data_pairs)
dataloader = DataLoader(dataset, batch_size=1, shuffle=True)

# Preview a batch
for x, y in dataloader:
    print("Input:", x)
    print("Output:", y)
    break
```
**Sample Output:**
```
Input: tensor([[ 8,  3,  0,  6]])
Output: tensor([2])
```

---

## Step 4: LSTM Model Architecture

### Components:
- `nn.Embedding`: Converts word indices to dense vectors
- `nn.LSTM`: Processes sequences with memory (cell + hidden)
- `nn.Linear`: Maps hidden state to output vocabulary size

```python
import torch.nn as nn

class LSTMModel(nn.Module):
    def __init__(self, vocab_size, embed_dim, hidden_dim):
        super(LSTMModel, self).__init__()
        self.embedding = nn.Embedding(vocab_size, embed_dim)
        self.lstm = nn.LSTM(embed_dim, hidden_dim, batch_first=True)
        self.fc = nn.Linear(hidden_dim, vocab_size)

    def forward(self, x):
        embed = self.embedding(x)
        out, (h_n, c_n) = self.lstm(embed)
        last_hidden = out[:, -1, :]
        out = self.fc(last_hidden)
        return out
```

---

## Step 5: Train the Model

```python
model = LSTMModel(vocab_size=len(vocab), embed_dim=10, hidden_dim=20)
criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=0.01)

for epoch in range(100):
    for x, y in dataloader:
        optimizer.zero_grad()
        output = model(x)
        loss = criterion(output, y)
        loss.backward()
        optimizer.step()
    if epoch % 10 == 0:
        print(f"Epoch {epoch}, Loss: {loss.item():.4f}")
```

---

## Step 6: Inference

```python
def predict(model, input_text):
    model.eval()
    tokens = [word2idx[w] for w in input_text.split()]
    x = torch.tensor(tokens).unsqueeze(0)
    with torch.no_grad():
        output = model(x)
        pred = torch.argmax(output, dim=1).item()
    return idx2word[pred]

print(predict(model, "what is capital of"))
```
**Sample Output:**
```
'india'
```

---

## 🔍 Explanation of Components

### What is `nn.LSTM`?
LSTM stands for **Long Short-Term Memory**, it maintains both **hidden state** and **cell state**.
- More capable of remembering long-term dependencies than basic RNN.

### What is `nn.Embedding`?
Maps token index to dense vector.
```python
input = torch.tensor([8, 3, 0, 6])
embed = model.embedding(input)
print(embed.shape)
```
Output:
```
torch.Size([4, 10])
```

### What is `nn.Linear`?
Final layer that maps LSTM output to vocab scores.

### What is `Softmax`?
`CrossEntropyLoss` uses softmax internally to calculate probabilities and loss.

---

## ✅ Summary

| Step | Description | Input | Output |
|------|-------------|--------|--------|
| Data Prep | Clean and tokenize | Text | Tokens |
| Vocab | Build vocab dicts | Tokens | word2idx, idx2word |
| Dataset | Create (X, y) pairs | Sentences | Token IDs |
| Model | Embedding + LSTM + FC | Token IDs | Prediction logits |
| Train | CrossEntropy Loss | Model output, true label | Trained weights |
| Predict | Use trained model | New input sequence | Predicted word |

---

This LSTM version mirrors the RNN steps, but with enhanced memory handling for better performance on sequential data.

