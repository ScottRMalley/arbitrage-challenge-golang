# Arbitrage Challenge Golang

Several new Automated Market Maker contracts have been released on Binance Smart Chain. There are two new decentralized
exchanges (DEXs) on this blockchain that each start out with a certain amount of liquidity and offer spicy hot trade
opportunities (JalepeñoSwap and ScorpionSwap). With some luck and skillful programming, hopefully you can manage to
extract some profit from these exchanges.

## Part 1: Simulating a Decentralized Exchange

The first step in setting up informed trades will be to use the information we have to calculate the prices and
liquidity from the swap events we have measured on-chain. These decentralized exchanges operate by use of the constant
product market maker function. This means that upon pool creation, liquidity providers deposit a certain amount of each
asset in the trade pair, and the product of these two amounts is kept constant when executing each swap.

For example, consider a DOGE/USDT pair on one of these DEXs. At time of
creation <img src="https://latex.codecogs.com/gif.latex?\inline&space;X_{DOGE}" title="X_{DOGE}" />
and <img src="https://latex.codecogs.com/gif.latex?\inline&space;Y_{USDT}" title="Y_{USDT}" /> token amounts are
deposited in the liquidity pool. The initial
constant <img src="https://latex.codecogs.com/gif.latex?\inline&space;K" title="K" /> is calculated via:

<p align="center">
<img src="https://latex.codecogs.com/gif.latex?X_{DOGE}\cdot&space;Y_{USDT}&space;=&space;K" title="X_{DOGE}\cdot Y_{USDT} = K" />
</p>

Future liquidity deposits will ensure a consistent ratio between the current DOGE and USDT amounts, however for the
purposes of this challenge, there are no further liquidity deposits in the pool. This means that the initial
constant <img src="https://latex.codecogs.com/gif.latex?\inline&space;K" title="K" /> will remain the same throughout
the exercise. In order to calculate the output amount if a user swaps DOGE for USDT, we need to calculate the new values
of  <img src="https://latex.codecogs.com/gif.latex?\inline&space;X" title="X" />
and <img src="https://latex.codecogs.com/gif.latex?\inline&space;Y" title="Y" /> that will
keep <img src="https://latex.codecogs.com/gif.latex?\inline&space;K" title="K" /> constant

<p align="center">
<img src="https://latex.codecogs.com/gif.latex?\begin{matrix}&space;X_0\cdot&space;Y_0&space;=&space;K\\&space;X_1\cdot&space;Y_1&space;=&space;K\\&space;X_1&space;=&space;X_0&space;&plus;&space;X_{in}\\&space;Y_1&space;=&space;Y_0&space;-&space;Y_{out}&space;\end{matrix}" title="\begin{matrix} X_0\cdot Y_0 = K\\ X_1\cdot Y_1 = K\\ X_1 = X_0 + X_{in}\\ Y_1 = Y_0 - Y_{out} \end{matrix}" />
</p>
Solving the above system of equations for the USDT output <img src="https://latex.codecogs.com/gif.latex?\inline&space;Y_{out}" title="Y_{out}" />, we end up with the following:

<p align="center">
<img src="https://latex.codecogs.com/gif.latex?Y_{out}&space;=&space;Y_0&space;-&space;\frac{K}{X_0&space;&plus;&space;X_{in}}" title="Y_{out} = Y_0 - \frac{K}{X_0 + X_{in}}" />
</p>

When the trade amount has a very small price impact (
ie. <img src="https://latex.codecogs.com/gif.latex?\inline&space;X_{in}<<X_0" title="X_{in}<<X_0" />), this equation
simplifies to:

<p align="center">
<img src="https://latex.codecogs.com/gif.latex?Y_{out}&space;=&space;\frac{Y_0}{X_0}Y_{in}" title="price eq" />
</p>

This means that we can define the price <img src="https://latex.codecogs.com/gif.latex?\inline&space;P_n" title="P_n" />
for one DOGE as:

<p align="center">
<img src="https://latex.codecogs.com/gif.latex?P_n&space;=&space;\frac{Y_n}{X_n}" title="price def" />
</p>

Use the relationship between input and output amounts and the given trade data to calculate the price sequences of each
asset based on the updated reserve amounts after each swap (note that the swaps will not have a small price impact).

What is the all time high (ATH) and all time low (ATL) for each asset listed on the spicy DEXs? Provide answers in
USDT (not all pairs have a quote asset in USDT, so you'll have to do some currency conversion. Luckily, you have info on
the exchange rates).

## Part 2: Fees

In real life, DEXs won't swap tokens for free. The above example does not take into account the fees users will be
charged for executing a swap on the corresponding DEX. In this case, the exchanges charge the following fees:

* JalepeñoSwap: 0.3% swap fee
* ScorpionSwap: 0.2% swap fee

These fees are subtracted by from the input
amount <img src="https://latex.codecogs.com/gif.latex?\inline&space;X_{in}" title="X_{in}" />, meaning that our new
equation for calculating the output becomes:

<p align="center">
<img src="https://latex.codecogs.com/gif.latex?Y_{out}&space;=&space;Y&space;-&space;\frac{K}{X&space;-&space;\gamma&space;X_{in}}" title="Y_{out} = Y - \frac{K}{X - \gamma X_{in}}" />
</p>

Where <img src="https://latex.codecogs.com/gif.latex?\inline&space;\gamma" title="gamma" /> is `1-swapFee`. Therefore
the values for the two exchanges are:

*
JalepeñoSwap: <img src="https://latex.codecogs.com/gif.latex?\inline&space;\gamma&space;=&space;0.997" title="gamma jalepeno" />
*
ScorpionSwap: <img src="https://latex.codecogs.com/gif.latex?\inline&space;\gamma&space;=&space;0.998" title="gamma scorpion" />

Note that these fees will decrease the value
of <img src="https://latex.codecogs.com/gif.latex?\inline&space;Y_{out}" title="Y_{out}" />  meaning
that <img src="https://latex.codecogs.com/gif.latex?\inline&space;K" title="K" /> will no longer stay exactly the same
between swaps. Ensure that when calculating the next swap, you are including an updated constant.

As we mentioned before, if you are performing a small swap that does not have a large price impact, the swap will be
what we found in part 1. This time however, instead of finding the max and min prices of each asset we want to calculate
the max amount that any swap paid for the given output
amount (<img src="https://latex.codecogs.com/gif.latex?\inline&space;Y_{out}" title="Y_{out}" />).

<p align="center">
<img src="https://latex.codecogs.com/gif.latex?P_{paid}&space;=&space;\frac{Y_{out}}{X_{in}}" title="P_{paid} = \frac{Y_{out}}{X_{in}}" />
</p>

Including fees, what is the highest amount that anyone paid for each asset listed on the exchanges? Again, provide
answers in USDT.

## Part 3: Arbitrage

You may have noticed that the resulting prices for each asset on the two exchanges do not always match up. This means
that sometimes an asset might be overpriced on a certain exchange, and underpriced on another.

When this happens, we can buy the asset on the underpriced exchange and sell in on the overpriced exchange to make a
profit. There are a few things to keep in mind:

* Exact output <img src="https://latex.codecogs.com/gif.latex?\inline&space;Y_{out}" title="Y_{out}" /> will not
  correspond exactly to the expected price and depends on the swap
  size <img src="https://latex.codecogs.com/gif.latex?\inline&space;X_{in}" title="X_{in}" />. Use the actual output to
  determine whether a trade is profitable.
* Exchanges will charge different fees and and these should be taken into account to determine if a swap is profitable
* Trades can happen at the same time point, so if you buy asset A for B at one exchange, you can then sell asset B for A
  at another exchange during the same time point.

Each transaction on the blockchain costs gas fees in addition to the fees charged by the exchange. The gas fees are
equal to:

```
gasFee = gasPrice * gas
```

For a swap on both exchanges, there is a constant `gas` amount necessary to execute the trade. This value is:

```
gas = 120000
```

Remember that this is the gas fee for each trade with an exchange, so if you make multiple trades as part of a single
arbitrage transaction, this gas amount will be charged for each trade.

Gas fees are charged in the native currency of the blockchain (in this case BNB), so you may have to convert the gas fee
to another asset using the live exchange rate in order to compare values and evaluate whether a trade is profitable.
The `gasPrice` however depends on network conditions. You can find the `gasPrice` for each time point
in `gas-prices.txt`.

You can start with your own balance of 100 BNB. Go through the time series and determine when an arbitrage swap would be
profitable. Calculate the profit from the trade but for simplicity, don't execute the trade on your simulated exchange (
as this would change the prices). Just keep track of the profit you would have made if the swap was executed, and the
new amount of capital you would have for the next swap.

Through arbitraging between these exchanges, and with starting capital of 100 BNB, what is the most profit you can earn
before the end of the available data? How does this profit compare to just holding the 100 BNB?

As before, calculate your net worth at the end of the data in USDT.

## Data Organization

The Data for this challenge is organized per trading pair and per DEX. For example, the file corresponding to the
BNB/USDT pair on ScorpionSwap contains the following:

```
R_a R_b
28626056806401828716544 2000000000000000000000000

time order amount currency
0 BUY 196752925820543172608 BNB
1 BUY 72810613505815126016 BNB
2 BUY 589952515473476550656 BNB
3 BUY 179293949844126466048 BNB
4 SELL 116306137184619724800 BNB
```

The first numbers `R_a` and `R_b` correspond to the initial supply of each token. In this case `R_a` is the initial
supply for BNB (<img src="https://latex.codecogs.com/gif.latex?\inline&space;X_{BNB}" title="X_{BNB}" />) and `R_b` is
the initial supply of USDT (<img src="https://latex.codecogs.com/gif.latex?\inline&space;X_{USDT}" title="X_{USDT}" />).

The second section corresponds to each trade executed on this DEX. The fields include:

* `time` column is a incrementally increasing integer and can be used to correlate trades between exchanges.
* `order` column corresponds to whether it is a `BUY` or `SELL` order
* `amount` column corresponds to the amount of the asset being bought or sold
* `currency` column is the currency being bought or sold (always the base currency)

**Note**: All currency units are in [gwei](https://academy.binance.com/en/glossary/wei), which is equal
to <img src="https://latex.codecogs.com/gif.latex?\inline&space;10^{-18}" title="10^{-18}" /> units of the corresponding
currency. This means that the following:

```
0 BUY 196752925820543172608 BNB
```

Represents a `BUY` of ~196.75 BNB.

## Further Resources

* [Jalepeño](https://en.wikipedia.org/wiki/Jalape%C3%B1o)
* [Trinidad Scorpion](https://en.wikipedia.org/wiki/Trinidad_Moruga_scorpion)
