# mask
mask style
１.padding mask
embedding = token embedding + positional embedding
so the embedding input is needed to multiply padding mask.
shape as [batch_size, len_sequence], or [batch_size,len_sequence,1]
[[1,1,1,0,0],[1,1,1,1,0]]
or 
[[[1]
  [1]
  [1]
  [0]
  [0]]

 [[1]
  [1]
  [1]
  [1]
  [0]]]
  
  2. encoder self-attention mask
  attention mask is square matric.
  such as: 'I like [pad]'.
  self-attention mask is :
  [[1, 1, 0],
  [1, 1, 0],
  [0, 0, 0]]
  3. decoder masked attention is a square matric = padding mask & low-triangle matric
  such as: 'I like [pad]'.
  low-triangle:
  [[1, 0, 0],
  [1, 1, 0],
  [1, 1, 1],
  padding mask:
  [[1],
  [1],
  [0]]
  The result is:
  [[1, 0, 0],
  [1, 1, 0],
  [0, 0, 0]]
  4. encoder-decoder attention:
  If the source sequence is 'I like [pad]'
  The attention is:
  [[1, 1, 0],
  [1, 1, 0],
  [1, 1, 0],
  [1, 1, 0]]
  The number of rows is the token number of target sequence.
  
  The last attention point is that dot product between querys and keys multiply zero mask firstly and then add the -inf attention before softmax operation.
  score *= score_mask     #score is the dot product, *score_mask is to delete the padding function
  score += ((score_mask-1) * 1e+10) # add -inf is to delete the "0" attention function between tokens in softmax operation
  
