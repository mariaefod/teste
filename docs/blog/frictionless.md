# Frictionless

poetry add frictionless

poetry add 'frictionless[cli]'

poetry add 'frictionless[sql]'

poetry add 'frictionless[excel]'

#

frictionless describe

frictionless explore

frictionless extract

frictionless index

frictionless list

frictionless publish

frictionless query

frictionless script

frictionless validate

frictionless --help

frictionless --version

#

```
poetry run frictionless extract data/servidores.csv

                        extract --resource-name servidores datapackage.yaml

           frictionless describe data/servidores.csv

                        describe data/servidores.csv --yaml > servidores.resource.yaml

                        describe data/servidores.csv --stats --json 

           frictionless validate data/servidores.csv

                        validate data/servidores.csv --path datapackage.yaml  

           frictionless

           frictionless convert data/servidores.csv --to-path servidores.xlsx

           frictionless explore data/servidores.csv

           frictionless list data/*.csv
```

#

frictionless transform countries.csv --pipeline countries.pipeline.yaml

frictionless index table.csv --database sqlite:///index/project.db    # tabela sql

frictionless publish data/tables/*.csv --target http://ckan:5000/dataset/my-best --title "My best dataset"


-----


O schema valida tipos, formatos e constraints declarados no datapackage.

Os checks validam regras extras, como:

- linhas duplicadas (--checks duplicate-row)

- número mínimo/máximo de linhas (--checks table-dimensions:numRows=43)

- chaves primárias

- problemas de header

- tabelas inconsistentes


-----
### python

```
from frictionless import describe, extract, validate, transform, steps

resource = describe('table.csv')
rows = extract('table.csv')
report = validate('table.csv')
resource = transform('table.csv', steps=[steps.cell_set(field_name='name', value='new')])
```

```
from frictionless import Package, Resource

package = Package(
    name='package',
    title='My Package',
    description='My Package for the Guide',
    resources=[Resource(path='table.csv')])
package = Package('datapackage.json') # from a descriptor

package = Package('datapackage.json')
package.name = 'new-name'
package.title = 'New Title'
package.description = 'New Description'

package.add_resource(Resource(name='new', data=[['key1', 'key2'], ['val1', 'val2']]))
resource = package.get_resource('new')
print(package.has_resource('new'))
package.remove_resource('new')

package.to_json('datapackage.json') # Save as JSON
package.to_yaml('datapackage.yaml') # Save as YAML

resource = Resource('table.csv') # from a resource path
resource = Resource('resource.json') # from a descriptor path
resource = Resource({'path': 'table.csv'}) # from a descriptor
resource = Resource(path='table.csv') # from arguments
```

```
from frictionless import Resource, Dialect

dialect = Dialect(header=False)
with Resource('capital-3.csv', dialect=dialect) as resource:
      print(resource.header.labels)
      print(resource.to_view())
```

```
from frictionless import Schema, fields, describe

schema = describe('table.csv', type='schema') # from a resource path
schema = Schema.from_descriptor('schema.json') # from a descriptor path
schema = Schema.from_descriptor({'fields': [{'name': 'id', 'type': 'integer'}]}) # from a descriptor
```

```
from frictionless import Checklist, checks

checklist = Checklist(checks=[checks.row_constraint(formula='id > 1')])
print(checklist)

class duplicate_row(Check):
    code = "duplicate-row"
    Errors = [errors.DuplicateRowError]

    def __init__(self, descriptor=None):
        super().__init__(descriptor)
        self.__memory = {}

    def validate_row(self, row):
        text = ",".join(map(str, row.values()))
        hash = hashlib.sha256(text.encode("utf-8")).hexdigest()
        match = self.__memory.get(hash)
        if match:
            note = 'the same as row at position "%s"' % match
            yield errors.DuplicateRowError.from_row(row, note=note)
        self.__memory[hash] = row.row_position

    metadata_profile = {  # type: ignore
        "type": "object",
        "properties": {},
    }
```

```
from frictionless import Detector, describe

detector = Detector(field_type='string')
resource = describe("country-1.csv", detector=detector)
print(resource.schema)
```

```
from frictionless import validate

report = validate('capital-invalid.csv', pick_errors=['duplicate-label'])
error = report.task.error # it's only available for 1 table / 1 error sitution
print(f'Type: "{error.type}"')
print(f'Title: "{error.title}"')
print(f'Tags: "{error.tags}"')
print(f'Note: "{error.note}"')
print(f'Message: "{error.message}"')
print(f'Description: "{error.description}"')
```

```
from frictionless import errors

class DuplicateRowError(errors.RowError):
    code = "duplicate-row"
    name = "Duplicate Row"
    tags = ["#table", "#row", "#duplicate"]
    template = "Row at position {rowPosition} is duplicated: {note}"
    description = "The row is duplicated."
```

```
from frictionless import Resource, transform, Pipeline, steps

source = Resource(path="transform.csv") # Define source resource

pipeline = Pipeline(steps=[ # Create a pipeline
    steps.table_normalize(),
    steps.field_add(name="cars", formula='population*2', descriptor={'type': 'integer'}),
])

target = source.transform(pipeline) # Apply transform pipeline

print(target.schema) # Print resulting schema and data
print(target.to_view())
```

-----

No .yaml

```
path: table.csv

checklist: checklist.yaml
pipeline: pipeline.yaml
```

No CLI

```
frictionless validate resource.yaml  # will use the checklist above
frictionless transform resource.yaml  # will use the pipeline above
```
