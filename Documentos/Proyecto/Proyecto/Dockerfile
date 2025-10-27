# --- Etapa 1: Compilar la Aplicación ---
# Usamos la imagen del SDK de .NET 8, que tiene todas las herramientas para construir.
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copia el archivo del proyecto y restaura las dependencias primero.
# Esto es un truco de caché de Docker para que las futuras compilaciones sean más rápidas.
# ¡IMPORTANTE! Reemplaza "Proyecto.csproj" con el nombre real de tu archivo .csproj
COPY ["Proyecto.csproj", "./"]
RUN dotnet restore "./Proyecto.csproj"

# Copia el resto del código fuente.
COPY . .
# Publica la aplicación en modo "Release", optimizado para producción.
RUN dotnet publish "Proyecto.csproj" -c Release -o /app/publish --no-restore

# --- Etapa 2: Crear la Imagen Final ---
# Usamos la imagen de ASP.NET Runtime, que es mucho más ligera porque no tiene las herramientas de compilación.
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
# Copia solo la aplicación compilada desde la etapa anterior.
COPY --from=build /app/publish .

# Render se conectará a este puerto. Kestrel (el servidor de .NET) escucha en el 8080 por defecto en los contenedores más nuevos.
EXPOSE 8080

# El comando que se ejecuta para iniciar tu aplicación.
# ¡IMPORTANTE! Reemplaza "Proyecto.dll" con el nombre real de la DLL de tu proyecto.
ENTRYPOINT ["dotnet", "Proyecto.dll"]