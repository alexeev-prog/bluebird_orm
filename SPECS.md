# BlueBird ORM Specification

## Simple usage

```python
from bluebird_orm.managers import Manager
from bluebird_orm.connectors import SQLiteConnector
from bluebird_orm.fields import IntField, StrField
from bluebird_orm.models import Model
from bluebird_orm.prototypes import ProtoModelType, ProtoTableType

table = ProtoTableType(id=IntField(primary_key=True), name=StrField(nullable=False))
protouser = ProtoModelType(prototype_model=table)

conn = SQLiteConnector(path="example.db")
manager = Manager(conn=conn)
manager.create_table("Users")
manager.insert(protouser.new(name="John Doe"))
manager.insert(protouser.new(name="Jane Doe"))
manager.commit()


class User(Model):
    __tablename__ = "Users"

    id: int = IntField(primary_key=True)
    name: str = StrField(nullable=False)


manager.insert(User)
manager.commit()
```

## Sessions

```python
from bluebird_orm.managers import Manager
from bluebird_orm.sessions import SessionFabric
from bluebird_orm.connectors import SQLiteConnector

conn = SQLiteConnector(path="example.db")
session_fabric = SessionFabric(connection=conn)
current_session = session_fabric.build_once()

manager = Manager(session=current_session)
```

