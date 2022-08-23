# tinybench

Benchmark your code easily with Tinybench, a simple, tiny and light-weight
benchmarking library!
You can run your benchmarks in multiple javascript runtimes, Tinybench is
completely based on the Web APIs with proper timing using `process.hrtime` or
`performance.now`.

- Accurate and precise timing based on the environment
- `Event` and `EventTarget` compatible events
- Statistically analyzed values
- Calculated Percentiles
- Fully detailed results

_In case you need more tiny libraries like tinypool or tinyspy, please consider submitting an [RFC](https://github.com/tinylibs/rfcs)_

## Installing

```bash
$ npm install -D tinybench
```

## Usage

You can start benchmarking by instantiating the `Bench` class and adding
benchmark tasks to it.

```ts
const bench = new Bench({ time: 100 });

bench
  .add("foo", () => {
    // code
  })
  .add("bar", async () => {
    // code
  });

await bench.run();
```

The `add` method accepts a task name and a task function, so it can benchmark
it! This method returns a reference to the Bench instance, so it's possible to
use it to create an another task for that instance.

Note that the task name should always be unique in an instance, because Tinybench stores the tasks based
on their names in a `Map`.

## Docs

### `Bench`

The Benchmark instance for keeping track of the benchmark tasks and controlling
them.

Options:

```ts
type Options = {
  /**
   * time needed for running a benchmark task (milliseconds)
   */
  time?: number;

  /**
   * function to get the current timestamp in milliseconds
   */
  now?: () => number;

  /**
   * An AbortSignal for aborting the benchmark
   */
  signal?: AbortSignal;
};
```

- `async run()`: run the added tasks that were registered using the `add` method
- `reset()`: reset each task and remove its result
- `add(name: string, fn: Fn)`: add a benchmark task to the task map
- - `Fn`: `() => any | Promise<any>`
- `remove(name: string)`: remove a benchmark task from the task map
- `get results(): (TaskResult | undefined)[]`: (getter) tasks results as an array
- `get tasks(): Task[]`: (getter) tasks as an array
- `getTask(name: string): Task | undefined`: get a task based on the name

### `Task`

A class that represents each benchmark task in Tinybench. It keeps track of the
results, name, Bench instance, the task function and the number of times the task
function has been executed.

- `constructor(bench: Bench, name: string, fn: Fn)`
- `bench: Bench`
- `name: string`: task name
- `fn: Fn`: the task function
- `runs: number`: the number of times the task function has been executed
- `result?: TaskResult`: the result object
- `async run()`: run the current task and write the results in `Task.result` object
- `setResult(result: Partial<TaskResult>)`: change the result object values
- `reset()`: reset the task to make the `Task.runs` a zero-value and remove the `Task.result` object

## `TaskResult`

the benchmark task result object.

```ts
type TaskResult = {
  /*
   * the last error that was thrown while running the task
   */
  error?: unknown;

  /**
   * The amount of time in milliseconds to run the benchmark task (cycle).
   */
  totalTime: number;

  /**
   * the minimum value in the samples
   */
  min: number;
  /**
   * the maximum value in the samples
   */
  max: number;

  /**
   * the number of operations per second
   */
  hz: number;

  /**
   * how long each operation takes (ms)
   */
  period: number;

  /**
   * task samples of each task iteration time (ms)
   */
  samples: number[];

  /**
   * samples mean/average (estimate of the population mean)
   */
  mean: number;

  /**
   * samples variance (estimate of the population variance)
   */
  variance: number;

  /**
   * samples standard deviation (estimate of the population standard deviation)
   */
  sd: number;

  /**
   * standard error of the mean (a.k.a. the standard deviation of the sampling distribution of the sample mean)
   */
  sem: number;

  /**
   * degrees of freedom
   */
  df: number;

  /**
   * critical value of the samples
   */
  critical: number;

  /**
   * margin of error
   */
  moe: number;

  /**
   * relative margin of error
   */
  rme: number;

  /**
   * p75 percentile
   */
  p75: number;

  /**
   * p99 percentile
   */
  p99: number;

  /**
   * p995 percentile
   */
  p995: number;

  /**
   * p999 percentile
   */
  p999: number;
};
```

## Authors

| <a href="https://github.com/Aslemammad"> <img width='150' src="https://avatars.githubusercontent.com/u/37929992?v=4" /><br> Mohammad Bagher </a> |
| ------------------------------------------------------------------------------------------------------------------------------------------------ |

## Credits

| <a href="https://github.com/uzploak"> <img width='150' src="https://avatars.githubusercontent.com/u/5059100?v=4" /><br> Uzlopak </a> | <a href="https://github.com/poyoho"> <img width='150' src="https://avatars.githubusercontent.com/u/36070057?v=4" /><br> poyoho </a> |
| ------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------- |
