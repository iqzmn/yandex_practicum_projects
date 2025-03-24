### **Project Goal**  

We have access to data from the **Yandex.Real Estate** service—an archive of apartment sale listings in **St. Petersburg and neighboring localities** over several years. Our task is to learn how to **determine the market value of real estate** based on the provided parameters. This will enable the development of an **automated system** that can also detect **market anomalies**.  

For each apartment listed for sale, two types of data are available:  
- **User-submitted data**  
- **Automatically generated data** based on cartographic information, such as **distance to the city center, airport, nearest park, or body of water**.  

### **The Provided Data Includes the Following Characteristics:**  
- `airports_nearest` — distance to the nearest airport (in meters)  
- `balcony` — number of balconies  
- `ceiling_height` — ceiling height (in meters)  
- `cityCenters_nearest` — distance to the city center (in meters)  
- `days_exposition` — number of days the listing was active (from posting to removal)  
- `first_day_exposition` — listing publication date  
- `floor` — apartment floor  
- `floors_total` — total number of floors in the building  
- `is_apartment` — whether the property is classified as an apartment (boolean)  
- `kitchen_area` — kitchen area (in square meters)  
- `last_price` — price at the time of listing removal  
- `living_area` — living area (in square meters)  
- `locality_name` — name of the locality  
- `open_plan` — whether the apartment has an open floor plan (boolean)  
- `parks_around3000` — number of parks within a 3 km radius  
- `parks_nearest` — distance to the nearest park (in meters)  
- `ponds_around3000` — number of bodies of water within a 3 km radius  
- `ponds_nearest` — distance to the nearest body of water (in meters)  
- `rooms` — number of rooms  
- `studio` — whether the apartment is a studio (boolean)  
- `total_area` — total apartment area (in square meters)  
- `total_images` — number of images in the listing  

### **Main Project Stages:**  
1. **Data Exploration**  
2. **Data Preprocessing**  
3. **Data Analysis**  
4. **Final Conclusion**  

### **Key Takeaways:**  
Real estate market analysis shows that **apartment prices depend on multiple factors**, which must be considered when buying or selling property.  

Some factors, such as **total area, number of rooms, and floor level**, are **fixed** and cannot be changed. Other factors, like **listing publication date and the need for renovations**, are **variable** and can be **leveraged to optimize pricing**.  

For a **successful transaction**, it is essential to **analyze real estate market data** and choose the **most suitable time** to list a property for sale.
