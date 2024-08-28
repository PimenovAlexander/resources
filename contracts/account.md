# Account

## Description

Account contract. This a contract that stores the funds that user would use for minting, And also proxies the minting message

## Data Storage

Location [accountv3](accountv3/)

<table><thead><tr><th width="88">Index</th><th width="78">Type</th><th width="96">Size (b/r)</th><th width="62">Cell</th><th width="199">Name</th><th>Description</th></tr></thead><tbody><tr><td>1</td><td>addr</td><td>267 / 0</td><td>1</td><td>account::user_address</td><td>Address of the user ton wallet of the user that owns this two jetton account</td></tr><tr><td>2</td><td>addr</td><td>267 / 0</td><td>1</td><td>account::pool_address</td><td>Address of the pool that created this two jetton account</td></tr><tr><td>3</td><td>coins</td><td>124 / 0</td><td>11</td><td>account::amount0</td><td>Anount of jetton0 (in pool terms) currently stored in the account</td></tr><tr><td>4</td><td>coins</td><td>124 / 0</td><td>11</td><td>account::amount1</td><td>Anount of jetton1 (in pool terms) currently stored in the account</td></tr><tr><td>5</td><td>coins</td><td>124 / 0</td><td>11</td><td>account::enough0</td><td>Anount of jetton0 (in pool terms) that is enought to make a mint operation. When reached for both tokens, mint is triggered</td></tr><tr><td>6</td><td>coins</td><td>124 / 0</td><td>11</td><td>account::enough1</td><td>Anount of jetton1 (in pool terms) that is enought to make a mint operation.</td></tr><tr><td>### Cells</td><td></td><td></td><td></td><td></td><td></td></tr><tr><td>Name</td><td>Size</td><td>Free</td><td></td><td></td><td></td></tr><tr><td>---</td><td>---</td><td>---</td><td></td><td></td><td></td></tr><tr><td>1</td><td>534</td><td>489</td><td></td><td></td><td></td></tr><tr><td>11</td><td>496</td><td>527</td><td></td><td></td><td></td></tr></tbody></table>
