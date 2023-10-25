> FLEDGE has been renamed to Protected Audience API. To learn more about the name change, see the [blog post](https://privacysandbox.com/intl/en_us/news/protected-audience-api-our-new-name-for-fledge)

# AboveThreshold with Periodic Restarts Algorithm

At a high level, the job of the k-anonymity server is to keep track, for each
"set" _S_, how many distinct users have invoked `Join` on _S_ over a given
lookback period _w_ (say, last 7 days). If the set size exceeds a predetermined
threshold _k_, the set is said to have k-anonymity status. The k-anonymity
server continually updates the k-anonymity status of each set known to it, and
publishes the status of each set via the `Query` endpoint.

In this document, we explain how the k-anonymity status of a set is updated and
published, to limit the ability of a bad actor to learn information about
individual user behavior by observing changes in the output of the Query
endpoint.

We employ the following techniques to protect user's privacy:


* The status of each set is only updated at periodic intervals (such as once an
hour) rather than continuously. This limits the ability of tying a specific set
status change (from no to yes) to a specific `Join` action based on timing.

* We add a calibrated amount of noise to both the true set size and the
k-anonymity threshold when evaluating the status of a set. This ensures
differential privacy properties for the algorithm and masks the contribution
of any single user's actions on the status of the set.

* We limit the frequency with which the status of a set can change. While it is
important to support fast ramp-ups (a large number of users Join-ing a set
should result in the set achieving k-anonymity quickly), we limit the speed at
which k-anonymity status is lost.


In [Differentially Private Algorithms for 𝑘-Anonymity Server][1] we analyze the
algorithm and formally bound the amount of information leakage.

We begin by describing the main [parameters](#parameters) of the algorithm and
presenting the [algorithm](#algorithm) itself at a high level. We then propose a list
of [initial parameter settings](#proposed-parameter-settings).

## Parameters

We will use the following parameters in the AboveThresholdWithPeriodicRestart
algorithm:

| Parameter   | Definition |
|-----------  |------------|
|$p$          | $p$ stands for the **query update period**. The $k$-anonymity query is evaluated at increments of $p$, that is, at approximate times $t_0$, $t_0+p$, $t_0+2p$... for some starting timestamp $t_0$. For convenience, we define the time values $(w, t)$ in multiples of $p$. For example, if $p$ is 1 hour, the window length will be an integer number of hours. |
|$w$          | We use $w$ for the set member **counting window length**. The window corresponds to a duration of $wp$ time. For a time $t\in \mathbb{N}$ (corresponding to timestamp $t_0+tp$), the set size subject to the $k$-anonymity requirement, is defined as the number of unique set members who joined the set in the interval $(t-w,t]$. We denote this count for the set $S$ by $C(S,t-w,t)$. |
|$k$          | We use $k$ for the **$k$-anonymity** threshold. The $k$-anonymity status of a set $S$ will be a noisy response to the query: does $S$ have at least $k$ members (i.e., is $C(S,t-w,t)\geq k)$? |
|$\varepsilon, \delta$ | $\varepsilon, \delta$ are the standard differential-privacy parameters. Let $f:\mathbb{N} \rightarrow {0 (false),1 (true)}$ (we can write $f\in2^\mathbb{N}$) be a function corresponding to a sequence of Query server responses for set $S$, where $t\in \mathbb{N}$. We denote this by $Query(S)=f$. We say that the query function is $(\varepsilon, \delta)$-private if for any $S$, any set $F\in 2^\mathbb{N}$ of possible Query server responses, and inputs $X, X'$ which are equal except for a single user $u$ addition to a set $S$, $P(Query(S)\in F\vert X)\leq e^\varepsilon P(Query(S)\in F\vert X')+\delta$. |

## The AboveThresholdWithPeriodRestartAlgorithm

The following paragraphs describe the algorithm at a high level. For further details,
see [Differentially Private Algorithms for 𝑘-Anonymity Server][1].

Let $A_t(S)$ be the evaluation of the $k$-anonymity query for set $S$ at time $t$.


**Parameters**: $p$ (Query update period); $w$ (counting window size), $k$ (threshold),
$\varepsilon, \delta$ (privacy parameters).

**Inputs**: Set $S$, $t$ (current time), $C(S,t-w,t)$ (exact count of unique clients
whose membership in $S$ overlaps with interval $(t-w,t]$).

**State**: $A_{t-1}(S)$ (most recent algorithm evaluation at time $t-1$)

**Algorithm $A_t(S)$**:

```
// algorithm restart
if t-1 = 0 mod w:
  state = false;
  // modify k with truncated Laplace noise mu (function of epsilon)
  generate k'=k+mu;
if state == true:
  A_t(S)=true;
else:
  generate nu;  // truncated Laplace noise
  A_t(S)=(C(S,t-w,t)+nu <= k');
```

## Proposed parameter settings

We propose the following parameter values to determine k-anonymity status of
a set.


| Parameter          | Value     |
|--------------------|-----------|
|Update period $p$   | $p=1$ hour. The k-anonymity server will re-evaluate the _k_-anonymity status of each set once every hour. |
|Lookback window $w$ | $w=30$ days (720 units of $p$). Join events from the past 30 days will count towards measuring the current set size. |
|Set size $k$        | $k=50$. A set must contain at least 50 users (in a noisy sense) to earn positive k-anonymity status. |


The differential privacy algorithm needs the $\varepsilon$ (epsilon) and
$\delta$ (delta) parameters to control the amount of noise added to the true
set size and the set size threshold. We choose the lowest round value of
$\varepsilon$ that permits $\delta \sim 10^{-5}$ given the choice of parameters
$p$, $w$, and $k$ above.


| Parameter            | Value     |
|----------------------|-----------|
|$\varepsilon, \delta$ | $\varepsilon=3$,  $\delta\sim 10^{-5}$ |


We verified that these parameters resulted in a limited amount of noise (as
shown in our document [Differentially Private Algorithms for 𝑘-Anonymity Server][1]).

[1]: DP_kanon_server.pdf
