### User Defined Functions

```python
Public Function aHouseSurfaceArea(HeightHouse As Double, WidthHouse As Double, DepthHouse As Double, HeightRoof As Double)

Dim Sides As Double
Dim FrontandBack As Double
Dim Roof As Double
Dim RoofSlant As Double


Sides = HeightHouse * DepthHouse * 2
FrontandBack = ((HeightHouse * WidthHouse) + (HeightRoof * WidthHouse / 2)) * 2
RoofSlant = Sqr(HeightRoof * HeightRoof + WidthHouse / 2 * WidthHouse / 2)

Roof = RoofSlant * DepthHouse * 2

HouseSurfaceArea = FrontandBack + Sides + Roof

End Function
```

Before

<img width="349" alt="Screen Shot 2022-04-13 at 4 09 04 PM" src="https://user-images.githubusercontent.com/73784742/163131230-b5ea6594-6d3d-48ae-bccc-05581c3d1f9e.png">

After

<img width="427" alt="Screen Shot 2022-04-13 at 4 09 37 PM" src="https://user-images.githubusercontent.com/73784742/163131295-9453a111-0a8c-4761-8af4-67e15e0d72a4.png">
