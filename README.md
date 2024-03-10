# pydantic-to-protobuf-bug

**Describe the bug**
When generating pydantic models for the [mapbox tile definition](https://github.com/mapbox/vector-tile-spec/blob/master/2.1/vector_tile.proto),
the following errors occur in the resulting model:

1. Feature and Value classes are duplicated outside of Tile class. They should not exist outside of the Tile class.
2. GeomType is incorrectly referenced without the Tile class prefix. 

**To Reproduce**
Steps to reproduce the behavior:
1. Run `python -m grpc_tools.protoc -I. --protobuf-to-pydantic_out=. vector_tile.proto`
2. Open "vector_tile_p2p.py"
3. See error

The repo located [here](https://github.com/bimpbomp/pydantic-to-protobuf-bug) reconstructs the scenario.

**Expected behavior**
1. The initial occurences of Feature and Value classes should not be present.
2. GeomType references should be Tile.GeomType

**Actual behaviour**
The following file is generated.
```
# This is an automatically generated file, please do not change
# gen by protobuf_to_pydantic[v0.2.5](https://github.com/so1n/protobuf_to_pydantic)
# Protobuf Version: 4.25.3 
# Pydantic Version: 2.6.3 
from enum import IntEnum
from google.protobuf.message import Message  # type: ignore
from pydantic import BaseModel
from pydantic import Field
import typing


    class Feature(BaseModel):

        id: int = Field(default=0) 
        tags: typing.List[int] = Field(default_factory=list) 
        type: GeomType = Field(default=0) 
        geometry: typing.List[int] = Field(default_factory=list) 

    class Value(BaseModel):

        string_value: str = Field(default="") 
        float_value: float = Field(default=0.0) 
        double_value: float = Field(default=0.0) 
        int_value: int = Field(default=0) 
        uint_value: int = Field(default=0) 
        sint_value: int = Field(default=0) 
        bool_value: bool = Field(default=False) 

class Tile(BaseModel):

    class Value(BaseModel):

        string_value: str = Field(default="") 
        float_value: float = Field(default=0.0) 
        double_value: float = Field(default=0.0) 
        int_value: int = Field(default=0) 
        uint_value: int = Field(default=0) 
        sint_value: int = Field(default=0) 
        bool_value: bool = Field(default=False) 
    class Feature(BaseModel):

        id: int = Field(default=0) 
        tags: typing.List[int] = Field(default_factory=list) 
        type: GeomType = Field(default=0) 
        geometry: typing.List[int] = Field(default_factory=list) 
    class Layer(BaseModel):

        version: int = Field(default=0) 
        name: str = Field(default="") 
        features: typing.List[Feature] = Field(default_factory=list) 
        keys: typing.List[str] = Field(default_factory=list) 
        values: typing.List[Value] = Field(default_factory=list) 
        extent: int = Field(default=0) 
    class GeomType(IntEnum):
        UNKNOWN = 0
        POINT = 1
        LINESTRING = 2
        POLYGON = 3



    layers: typing.List[Layer] = Field(default_factory=list) 

```

**Environment**
 - OS: Manjaro 
 - Python version: 3.11.6
 - protobuf==4.25.3
 - mypy-protobuf==3.5.0
 - protobuf-to-pydantic==0.2.5
