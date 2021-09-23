class: center, middle

# ExecNodes

---

# Agenda

- Motivation
- Introduction to the `class`es
- Building and running an ExecPlan
- FAQ
- Eventually...

---

# Motivation


We need a way to specify arbitrarily complex computations
(queries) with reusable components.

One very common way to structure this is by defining a data
flow graph: data flows along edges and is processed at nodes.


---

# Motivation

<svg width="320pt" height="404pt"
 viewBox="0.00 0.00 388.02 404.00" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
<g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 400)">
<title>G</title>
<polygon fill="#ffffff" stroke="transparent" points="-4,4 -4,-400 384.0173,-400 384.0173,4 -4,4"/>
<!-- scan lineitem -->
<g id="node1" class="node">
<title>scan lineitem</title>
<ellipse fill="none" stroke="#000000" cx="62.2569" cy="-378" rx="62.0148" ry="18"/>
<text text-anchor="middle" x="62.2569" y="-373.8" font-family="Times,serif" font-size="14.00" fill="#000000">scan lineitem</text>
</g>
<!-- filter -->
<g id="node2" class="node">
<title>filter</title>
<ellipse fill="none" stroke="#000000" cx="86.2569" cy="-306" rx="29.6089" ry="18"/>
<text text-anchor="middle" x="86.2569" y="-301.8" font-family="Times,serif" font-size="14.00" fill="#000000">filter</text>
</g>
<!-- scan lineitem&#45;&gt;filter -->
<g id="edge1" class="edge">
<title>scan lineitem&#45;&gt;filter</title>
<path fill="none" stroke="#000000" d="M68.3132,-359.8314C70.9767,-351.8406 74.163,-342.2819 77.1065,-333.4514"/>
<polygon fill="#000000" stroke="#000000" points="80.4439,-334.5071 80.2858,-323.9134 73.8031,-332.2934 80.4439,-334.5071"/>
</g>
<!-- join -->
<g id="node3" class="node">
<title>join</title>
<ellipse fill="none" stroke="#000000" cx="184.2569" cy="-234" rx="27" ry="18"/>
<text text-anchor="middle" x="184.2569" y="-229.8" font-family="Times,serif" font-size="14.00" fill="#000000">join</text>
</g>
<!-- filter&#45;&gt;join -->
<g id="edge2" class="edge">
<title>filter&#45;&gt;join</title>
<path fill="none" stroke="#000000" d="M105.6186,-291.7751C120.5341,-280.8168 141.3184,-265.5467 157.7735,-253.4572"/>
<polygon fill="#000000" stroke="#000000" points="159.9433,-256.2062 165.9299,-247.4648 155.7988,-250.565 159.9433,-256.2062"/>
</g>
<!-- join again -->
<g id="node4" class="node">
<title>join again</title>
<ellipse fill="none" stroke="#000000" cx="231.2569" cy="-162" rx="49.2784" ry="18"/>
<text text-anchor="middle" x="231.2569" y="-157.8" font-family="Times,serif" font-size="14.00" fill="#000000">join again</text>
</g>
<!-- join&#45;&gt;join again -->
<g id="edge3" class="edge">
<title>join&#45;&gt;join again</title>
<path fill="none" stroke="#000000" d="M195.1578,-217.3008C200.8051,-208.6496 207.8305,-197.8873 214.1788,-188.1623"/>
<polygon fill="#000000" stroke="#000000" points="217.224,-189.9002 219.7594,-179.6132 211.3623,-186.0738 217.224,-189.9002"/>
</g>
<!-- filter again -->
<g id="node9" class="node">
<title>filter again</title>
<ellipse fill="none" stroke="#000000" cx="231.2569" cy="-90" rx="53.2645" ry="18"/>
<text text-anchor="middle" x="231.2569" y="-85.8" font-family="Times,serif" font-size="14.00" fill="#000000">filter again</text>
</g>
<!-- join again&#45;&gt;filter again -->
<g id="edge8" class="edge">
<title>join again&#45;&gt;filter again</title>
<path fill="none" stroke="#000000" d="M231.2569,-143.8314C231.2569,-136.131 231.2569,-126.9743 231.2569,-118.4166"/>
<polygon fill="#000000" stroke="#000000" points="234.757,-118.4132 231.2569,-108.4133 227.757,-118.4133 234.757,-118.4132"/>
</g>
<!-- scan orders -->
<g id="node5" class="node">
<title>scan orders</title>
<ellipse fill="none" stroke="#000000" cx="197.2569" cy="-378" rx="54.9752" ry="18"/>
<text text-anchor="middle" x="197.2569" y="-373.8" font-family="Times,serif" font-size="14.00" fill="#000000">scan orders</text>
</g>
<!-- project -->
<g id="node6" class="node">
<title>project</title>
<ellipse fill="none" stroke="#000000" cx="184.2569" cy="-306" rx="37.6986" ry="18"/>
<text text-anchor="middle" x="184.2569" y="-301.8" font-family="Times,serif" font-size="14.00" fill="#000000">project</text>
</g>
<!-- scan orders&#45;&gt;project -->
<g id="edge4" class="edge">
<title>scan orders&#45;&gt;project</title>
<path fill="none" stroke="#000000" d="M193.9765,-359.8314C192.5861,-352.131 190.9329,-342.9743 189.3877,-334.4166"/>
<polygon fill="#000000" stroke="#000000" points="192.8028,-333.6322 187.5816,-324.4133 185.9142,-334.8761 192.8028,-333.6322"/>
</g>
<!-- project&#45;&gt;join -->
<g id="edge5" class="edge">
<title>project&#45;&gt;join</title>
<path fill="none" stroke="#000000" d="M184.2569,-287.8314C184.2569,-280.131 184.2569,-270.9743 184.2569,-262.4166"/>
<polygon fill="#000000" stroke="#000000" points="187.757,-262.4132 184.2569,-252.4133 180.757,-262.4133 187.757,-262.4132"/>
</g>
<!-- scan customers -->
<g id="node7" class="node">
<title>scan customers</title>
<ellipse fill="none" stroke="#000000" cx="310.2569" cy="-306" rx="69.5216" ry="18"/>
<text text-anchor="middle" x="310.2569" y="-301.8" font-family="Times,serif" font-size="14.00" fill="#000000">scan customers</text>
</g>
<!-- aggregate -->
<g id="node8" class="node">
<title>aggregate</title>
<ellipse fill="none" stroke="#000000" cx="294.2569" cy="-234" rx="48.6346" ry="18"/>
<text text-anchor="middle" x="294.2569" y="-229.8" font-family="Times,serif" font-size="14.00" fill="#000000">aggregate</text>
</g>
<!-- scan customers&#45;&gt;aggregate -->
<g id="edge6" class="edge">
<title>scan customers&#45;&gt;aggregate</title>
<path fill="none" stroke="#000000" d="M306.2195,-287.8314C304.5083,-280.131 302.4735,-270.9743 300.5717,-262.4166"/>
<polygon fill="#000000" stroke="#000000" points="303.9348,-261.4159 298.3488,-252.4133 297.1015,-262.9344 303.9348,-261.4159"/>
</g>
<!-- aggregate&#45;&gt;join again -->
<g id="edge7" class="edge">
<title>aggregate&#45;&gt;join again</title>
<path fill="none" stroke="#000000" d="M279.0064,-216.5708C271.1906,-207.6385 261.5369,-196.6056 252.9595,-186.8029"/>
<polygon fill="#000000" stroke="#000000" points="255.5861,-184.4897 246.367,-179.2687 250.3181,-189.0993 255.5861,-184.4897"/>
</g>
<!-- write to disk -->
<g id="node10" class="node">
<title>write to disk</title>
<ellipse fill="none" stroke="#000000" cx="231.2569" cy="-18" rx="59.1276" ry="18"/>
<text text-anchor="middle" x="231.2569" y="-13.8" font-family="Times,serif" font-size="14.00" fill="#000000">write to disk</text>
</g>
<!-- filter again&#45;&gt;write to disk -->
<g id="edge9" class="edge">
<title>filter again&#45;&gt;write to disk</title>
<path fill="none" stroke="#000000" d="M231.2569,-71.8314C231.2569,-64.131 231.2569,-54.9743 231.2569,-46.4166"/>
<polygon fill="#000000" stroke="#000000" points="234.757,-46.4132 231.2569,-36.4133 227.757,-46.4133 234.757,-46.4132"/>
</g>
</g>
</svg>



???

```
digraph G {

  "scan lineitem"
    -> "filter"
    -> "join"
    -> "join again";

  "scan orders"
    -> "project"
    -> "join";

  "scan customers"
    -> "aggregate"
    -> "join again"
    -> "filter again"
    -> "write to disk";

}
```

---

# The `class`es

quick overview:

```
// each node in the graph is an implementation of ExecNode
class ExecNode;

// all the nodes in the graph are owned by a single ExecPlan
class ExecPlan;

// nodes are constructed by factories in an ExecFactoryRegistry
class ExecFactoryRegistry;

// nodes are parameterized by ExecNodeOptions
class ExecNodeOptions;
```

---

# The `class`es

```md
class ExecNode;
class ExecPlan;
class ExecFactoryRegistry;
class ExecNodeOptions;
```

Before we go further: what's a batch?

```
// - lightweight
// - for use in compute:: only.
// - supports scalar "columns" (a single literal value)
// - has execution-relevant properties like guarantees
namespace arrow::compute {
  struct ExecBatch;
}

// - abstract base class (interface)
// - very public, exposed in all bindings
// - may eventually lazily materialize its columns for dataframiness
namespace compute {
  class RecordBatch;
}
```

These are interconvertible to each other (and to/from `cudf::table`).

???

- arrow::compute::ExecBatch is intended to be an extremely lightweight chunk of data, cheaply transmissible along edges of a compute graph. They support columns which are Scalars (a single literal value) in addition to Arrays, and carry execution-relevant sidecar properties like a guaranteed-true-filter for Expression simplification and a SelectionVector indicating delayed filtration.
- arrow::RecordBatch is an abstract base class. Although there is currently only one implementation (SimpleRecordBatch) there may eventually be fancier ones, such as one which lazily materializes its columns. It is much less cheap. arrow::Tables are also abstract base classes with the same motivation and unlike batches their columns are not required to be a single chunk of array.

---

# The `class`es - `ExecNode`

`src/arrow/compute/exec/exec_plan.h`

while the graph is running, instances of ExecNode:

1. receive batches from inputs and process them
  - unless it's a source
2. emit batches to outputs
  - unless it's a sink

???

maps onto a Blazing kernel

    - receives batches (unless it's a source) then does something then
      emits batches (unless it's a sink)
      - for example a FilterNode receives batches, drops some
        rows, then passes it along
      - for example an AggregateNode receives batches, updates
        sums, then emits results when the input completes
    - note that it's a graph node: you can look at the inputs
      and outputs vector to walk upstream or downstream in the
      graph
    - lifecycle methods:
      - need explicit start from outside the graph
      - can be stopped from outside the graph (for example, if the
      - can be stopped by an output (which doesn't need more batches)
      - error reporting != stopping; errors pass through the graph
        alongside batches instead of breaking everything.
    - note that each ExecNode has a single output schema

---

# The `class`es - `ExecPlan`

- keeps ExecNodes alive
- provides convenience methods for `"start all nodes!"`,
  `"stop all nodes!"`, and `"have all nodes completed?"`

```
// construct an ExecPlan first to hold your nodes
std::shared_ptr<ExecPlan> plan = *ExecPlan::Make(default_exec_context());

// add nodes to your ExecPlan
// ... (coming soon)

// start all nodes in the graph
ASSERT_OK(plan->StartProducing());

if (need_stop) {
  plan->StopProducing();
}

// wait for the plan to finish
plan->finished()->Wait();
```

???

Note: R users will need to use the gc_exec_context() instead
or risk spurious OutOfMemory errors.

---

# The `class`es - `ExecFactoryRegistry`

None of the concrete implementations of `ExecNode` are exposed
in headers, so they can't be constructed directly outside the
Translation Unit where they are defined.
Instead, factories to create them are added to a registry.

```
// get the factory for "filter" nodes:
auto make_filter = *default_exec_factory_registry()->GetFactory("filter");

// factories take three arguments:
ExecNode* filter_node = *make_filter(
  // the ExecPlan which should own this node
  plan.get(),

  // nodes which will send batches to this node (inputs)
  {scan_node},

  // parameters unique to "filter" nodes
  FilterNodeOptions{filter_expression});
```

???

"why a registry?"

- Decouple implementations from consumers of the interface
- Optimization
- Easier composition with out-of-library extensions like "scan" nodes
- More consitent construction for the bindings

---

# The `class`es - `ExecFactoryRegistry`

None of the concrete implementations of `ExecNode` are exposed
in headers, so they can't be constructed directly outside the
Translation Unit where they are defined.
Instead, factories to create them are added to a registry.

#### Don't use factories directly; we have helper functions:

```
ExecNode* filter_node = *MakeExecNode("filter",

  // the ExecPlan which should own this node
  plan.get(),

  // nodes which will send batches to this node (inputs)
  {scan_node},

  // parameters unique to "filter" nodes
  FilterNodeOptions{filter_expression});
```

---

# How to build an ExecPlan

```
// construct an ExecPlan first to hold your nodes
std::shared_ptr<ExecPlan> plan = *ExecPlan::Make(default_exec_context());

// add a source node which pulls batches from a RecordBatchReader
std::shared_ptr<RecordBatchReader> reader = GetStreamOfBatches();
ExecNode* source_node = *MakeExecNode("source",
                                      plan.get(),
                                      {},
                                      SourceNodeOptions::FromReader(
                                          reader,
                                          GetCpuThreadPool()));

// add a filter node which excludes rows from source_node's batches
ExecNode* filter_node = *MakeExecNode("filter",
                                      plan.get(),
                                      {source_node},
                                      FilterNodeOptions{
                                        greater(field_ref("score"), literal(3))
                                      });

// add a project node which materializes new columns based on filter_node's output
ExecNode* project_node = *MakeExecNode("project", /*...*/);

// add a sink node which collects the batches produced by the pipeline
ExecNode* sink_node = *MakeExecNode("sink", /*...*/);
```

.footnote[FromReader is in [PR#11032](https://github.com/apache/arrow/pull/11032) at the moment, I've included it here for brevity]

---

# How to build an ExecPlan

```
// construct an ExecPlan first to hold your nodes
std::shared_ptr<ExecPlan> plan = *ExecPlan::Make(default_exec_context());

// add a source node which pulls batches from a RecordBatchReader
ExecNode* source_node = *MakeExecNode("source", /* ... */);

// add a filter node which excludes rows from source_node's batches
ExecNode* filter_node = *MakeExecNode("filter", /* ... */);

// add a project node which materializes a new column from filter_node's output
ExecNode* project_node = *MakeExecNode("project",
                                       plan.get(),
                                       {filter_node},
                                       ProjectNodeOptions{
                                         {add(field_ref("score"), literal(1))},
                                         {"score + 1"}
                                       });

// add a sink node which collects the batches produced by the pipeline
std::shared_ptr<RecordBatchReader> sink_reader;
ExecNode* sink_node = *MakeExecNode("sink",
                                    plan.get(),
                                    {project_node},
                                    SinkNodeOptions::IntoReader(&sink_reader));
```

.footnote[IntoReader is not in any PR yet]

---

# How to build an ExecPlan

And for even less boilerplate, we have dplyr-inspired `Declaration`s:

```
// construct an ExecPlan first to hold your nodes
std::shared_ptr<ExecPlan> plan = *ExecPlan::Make(default_exec_context());

std::shared_ptr<RecordBatchReader> reader = GetStreamOfBatches(), sink_reader;
ASSERT_OK(Declaration::Sequence(
              {
                  {"source", SourceNodeOptions::FromReader(
                       reader,
                       GetCpuThreadPool()))},
                  {"filter", FilterNodeOptions{
                       greater(field_ref("score"), literal(3))}},
                  {"project", ProjectNodeOptions{
                       {add(field_ref("score"), literal(1))},
                       {"score + 1"}}},
                  {"sink", SinkNodeOptions::IntoReader(&sink_reader)},
              })
              .AddToPlan(plan.get()));
```

---

# Agenda

4. Gotchas
  - Not repeatably usable (constrast datasets, tables. Maps to
    RecordBatchReader, and in fact we can produce a source node
    which wraps a reader and we can extract a reader from a sink node)
  - Ordering, please
    - deterministic, as-on-disk ordering for simple scans
    - order dependent aggregates/window functions
  - How do these relate to Kernels?
5. Eventually...
  - Taskify
  - Backpressure
  - fan out execution of expressions
  - Maybe allow ordering along some edges of the graph
  - Maybe multiple outputs
  - No more explicit construction. *Everybody* should produce IR
    and have that mapped onto ExecNodes

---

# How to build an ExecPlan

```
TEST(ScanNode, MinimalGroupedAggEndToEnd) {
  // NB: This test is here for didactic purposes

* // Specify a MemoryPool and ThreadPool for the ExecPlan
  compute::ExecContext exec_context(default_memory_pool(), GetCpuThreadPool());

  // ensure arrow::dataset node factories are in the registry
  arrow::dataset::internal::Initialize();

  // A ScanNode is constructed from an ExecPlan (into which it is inserted),
  // a Dataset (whose batches will be scanned), and ScanOptions (to specify a filter for
  // predicate pushdown, a projection to skip materialization of unnecessary columns, ...)
  ASSERT_OK_AND_ASSIGN(std::shared_ptr<compute::ExecPlan> plan,
                       compute::ExecPlan::Make(&exec_context));

  std::shared_ptr<Dataset> dataset = std::make_shared<InMemoryDataset>(
      TableFromJSON(schema({field("a", int32()), field("b", boolean())}),
                    {
                        R"([{"a": 1,    "b": null},
                            {"a": 2,    "b": true}])",
                        R"([{"a": null, "b": true},
                            {"a": 3,    "b": false}])",
                        R"([{"a": null, "b": true},
                            {"a": 4,    "b": false}])",
                        R"([{"a": 5,    "b": null},
                            {"a": 6,    "b": false},
                            {"a": 7,    "b": false}])",
                    }));

  auto options = std::make_shared<ScanOptions>();
  // sync scanning is not supported by ScanNode
  options->use_async = true;
  // specify the filter
  compute::Expression b_is_true = field_ref("b");
  options->filter = b_is_true;
  // for now, specify the projection as the full project expression (eventually this can
  // just be a list of materialized field names)
  compute::Expression a_times_2 = call("multiply", {field_ref("a"), literal(2)});
  compute::Expression b = field_ref("b");
  options->projection =
      call("make_struct", {a_times_2, b}, compute::MakeStructOptions{{"a * 2", "b"}});

  // construct the scan node
  ASSERT_OK_AND_ASSIGN(
      compute::ExecNode * scan,
      compute::MakeExecNode("scan", plan.get(), {}, ScanNodeOptions{dataset, options}));

  // pipe the scan node into a project node
  ASSERT_OK_AND_ASSIGN(
      compute::ExecNode * project,
      compute::MakeExecNode("project", plan.get(), {scan},
                            compute::ProjectNodeOptions{{a_times_2, b}, {"a * 2", "b"}}));

  // pipe the projection into a grouped aggregate node
  ASSERT_OK_AND_ASSIGN(
      compute::ExecNode * aggregate,
      compute::MakeExecNode("aggregate", plan.get(), {project},
                            compute::AggregateNodeOptions{
                                {compute::internal::Aggregate{"hash_sum", nullptr}},
                                /*targets=*/{"a * 2"},
                                /*names=*/{"sum(a * 2)"},
                                /*keys=*/{"b"}}));

  // finally, pipe the aggregate node into a sink node
  AsyncGenerator<util::optional<compute::ExecBatch>> sink_gen;
  ASSERT_OK_AND_ASSIGN(compute::ExecNode * sink,
                       compute::MakeExecNode("sink", plan.get(), {aggregate},
                                             compute::SinkNodeOptions{&sink_gen}));

  ASSERT_THAT(plan->sinks(), ElementsAre(sink));

  // translate sink_gen (async) to sink_reader (sync)
  std::shared_ptr<RecordBatchReader> sink_reader = compute::MakeGeneratorReader(
      schema({field("sum(a * 2)", int64()), field("b", boolean())}), std::move(sink_gen),
      exec_context.memory_pool());

  // start the ExecPlan
  ASSERT_OK(plan->StartProducing());

  // collect sink_reader into a Table
  ASSERT_OK_AND_ASSIGN(auto collected, Table::FromRecordBatchReader(sink_reader.get()));

  // Sort table
  ASSERT_OK_AND_ASSIGN(
      auto indices, compute::SortIndices(
                        collected, compute::SortOptions({compute::SortKey(
                                       "sum(a * 2)", compute::SortOrder::Ascending)})));
  ASSERT_OK_AND_ASSIGN(auto sorted, compute::Take(collected, indices));

  // wait 1s for completion
  ASSERT_TRUE(plan->finished().Wait(/*seconds=*/1)) << "ExecPlan didn't finish within 1s";

  auto expected = TableFromJSON(
      schema({field("sum(a * 2)", int64()), field("b", boolean())}), {
                                                                         R"JSON([
                                               {"sum(a * 2)": 4,  "b": true},
                                               {"sum(a * 2)": 12, "b": null},
                                               {"sum(a * 2)": 40, "b": false}
                                          ])JSON"});
  AssertTablesEqual(*expected, *sorted.table(), /*same_chunk_layout=*/false);
}
```
