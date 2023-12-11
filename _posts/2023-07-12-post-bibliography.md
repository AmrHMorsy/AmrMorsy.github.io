---
layout: post
title: Rendering Spheres in C++ and OpenGL
date: 2023-12-07 00:00:00-0400
description:
tags:
categories:
giscus_comments: false
related_posts: false
related_publications:
---

By definition, a sphere is a 3D surface, where every vertex on the sphere is at the same distance from a given point. This given point is called the centre of the sphere, and this distance is called the radius of the sphere.


The equation of a sphere is: 


<div align="center"><b>$$ (x+c_x)^2 + (y+c_y)^2 + (z+c_z)^2 = r^2 $$</b></div>

<br> 
where r is the radius of the sphere, and C is the centre of the sphere. 


To render a sphere in OpenGL, we need vertices. There are infinite number of vertices in a sphere. We cannot render all of them. Instead, we will only render a limited number of vertices. <br>


To sample these vertices, we will divide the sphere into sectors and stacks. 
<br><br>
{% include figure.html path="assets/img/1.jpg" class="img-fluid rounded z-depth-1" %}
<br>

The points of intersection between each stack and each sector will be the sample vertices we will render, as shown below. 
<br><br>
{% include figure.html path="assets/img/2.jpg" class="img-fluid rounded z-depth-1" %}
<br>
The higher the number of sectors and stacks, the higher the number of sampled vertices and the more detailed the rendered sphere is. 

The vertex v can be computed by using parametric equations with the corresponding sector angle s and stack angle t. 
<br><br>
{% include figure.html path="assets/img/4.jpg" class="img-fluid rounded z-depth-1" %}
<br>
But, before we can compute v, we, first, have to compute the sector angle s and the stack angle t. 

We know that the sector angles ranges from 0 to 360 degrees. We also know that the stack angle ranges from -90 to 90 degrees. Hence, the sector angle s and the stack angle t at each step can be calculated as follows: 
<br><br>
<div align="center"><b>$$ s = 2*𝝅 * (Sector Index/Number of Sectors) $$</b></div>
<div align="center"><b>$$ t = (𝝅/2) - (𝝅*Stack Index/Number of Stacks) $$</b></div>
<br>

Using the sector angle s and the stack angle t, we can calculate the vertex v using the following equations: 
<br><br>
<div align="center"><b>$$ V.x = r * cos(t) * cos(s) $$</b></div>
<div align="center"><b>$$ V.y = r * cos(t) * sin(s) $$</b></div>
<div align="center"><b>$$ V.z = r * sin(t) $$</b></div>
<br>

Now that we have the vertices data, the next task is to triangulate adjacent vertices to form triangles. 

As shown below, each sector in a stack will consists of two triangles: ( V1 - V2 - V1+1 ) and ( V1+1 - V2 - V2+1 ). The only exceptions are the top (first) and the bottom (last) stacks: They will consist of only one triangle. 

Using this triangulation pattern, we can form the indices data that instructs OpenGL the indices of the vertices that form each triangle of the sphere. 
<br><br>
{% include figure.html path="assets/img/8.jpg" class="img-fluid rounded z-depth-1" %}
<br>

Next, we need to calculate the normal of each vertex. The normal data is one of the most important data needed to perform shading and lighting calculations. The calculation of the normal vector is simple: The normal vector is the perpendicular vector to the surface of the sphere at a given vertex. For a sphere centered at a point $C = (C_x, C_y, C_z)$ and a vertex $V = (x,y,z)$, the normal vector $N$ is the normalized vector pointing from the center $C$ to the vertex $V$. Mathematically, this is expressed as: 

$$N = normalize(V - C)$$

Normalizing the vector ensures that its length is one, a crucial aspect for accurate lighting and shading effects in OpenGL rendering. 

<hr>
<br>
The following is a C++ function that generates the vertices of the sphere. <br><br>


```c++
std::vector<float> generate_sphere_vertices( float radius, int number_of_sectors, int number_of_stacks )
{
    for( int i = 0 ; i <= number_of_stacks ; i++ ){
        float t = M_PI / 2 - i * (M_PI / number_of_stacks) ;
        float z = radius * sinf(t) ;
        for(int j = 0; j <= number_of_sectors ; ++j) {
            float s = j * (2 * M_PI / number_of_sectors) ;
            float x = radius * cosf(t) * cosf(s) ;
            float y = radius * cosf(t) * sinf(s) ;
            vertices.push_back(x) ;
            vertices.push_back(y) ;
            vertices.push_back(z) ;
        }
    }
    return vertices ;
}
```
<br>
The following is a C++ function that generates the indices of the sphere. <br><br>


```c++
std::vector<unsigned int> generate_sphere_indices( int number_of_sectors, int number_of_stacks ) 
{
    std::vector<unsigned int> indices ;
    for( int i = 0 ; i < number_of_stacks ; i++ ){
        int v1 = i * (number_of_sectors + 1) ;
        int v2 = v1 + number_of_sectors + 1 ;
        for( int j = 0 ; j < number_of_sectors ; j++ ) {
            if(i != 0){
                indices.push_back(v1) ;
                indices.push_back(v2) ;
                indices.push_back(v1 + 1) ; 
            }
            if(i != (number_of_stacks - 1)){
                indices.push_back(v1 + 1) ;
                indices.push_back(v2) ;
                indices.push_back(v2 + 1) ;
            }
            v1++ ; 
            v2++ ;
        }
    }
    return indices;
}
```

<br>

The following is the complete implementation to render a rotating sphere using C++ and OpenGL. <br><br>

```c++
#include <GL/glew.h>
#include <GLFW/glfw3.h>
#include <vector>
#include <cmath>
#include <iostream>
#include <glm/glm.hpp>
#include <glm/gtc/matrix_transform.hpp>
#include <glm/gtc/type_ptr.hpp>


const char * get_vertex_shader_source()
{
    return
    "#version 330 core \n"
    "in vec3 position; \n"
    "uniform mat4 model ; \n"
    "uniform mat4 view ; \n"
    "uniform mat4 projection ; \n"
    "void main() { \n"
    "    gl_Position = projection * view * model * vec4(position, 1.0); \n"
    "} \n" ;
}

const char * get_fragment_shader_source()
{
    return
    "#version 330 core \n"
    "out vec4 outColor; \n"
    "void main() { \n"
    "    outColor = vec4(1.0, 0.0, 0.0, 1.0); \n"
    "} \n" ;
}

std::vector<unsigned int> generate_sphere_indices( int number_of_sectors, int number_of_stacks )
{
    std::vector<unsigned int> indices ;
    for( int i = 0 ; i < number_of_stacks ; i++ ){
        int v1 = i * (number_of_sectors + 1) ;
        int v2 = v1 + number_of_sectors + 1 ;
        for( int j = 0 ; j < number_of_sectors ; j++ ) {
            if(i != 0){
                indices.push_back(v1) ;
                indices.push_back(v2) ;
                indices.push_back(v1 + 1) ;
            }
            if(i != (number_of_stacks - 1)){
                indices.push_back(v1 + 1) ;
                indices.push_back(v2) ;
                indices.push_back(v2 + 1) ;
            }
            v1++ ;
            v2++ ;
        }
    }
    return indices;
}

std::vector<float> generate_sphere_vertices( float radius, int number_of_sectors, int number_of_stacks )
{
    std::vector<float> vertices ;
    for( int i = 0 ; i <= number_of_stacks ; i++ ){
        float t = M_PI / 2 - i * (M_PI / number_of_stacks) ;
        float z = radius * sinf(t) ;
        for(int j = 0; j <= number_of_sectors ; ++j) {
            float s = j * (2 * M_PI / number_of_sectors) ;
            float x = radius * cosf(t) * cosf(s) ;
            float y = radius * cosf(t) * sinf(s) ;
            vertices.push_back(x) ;
            vertices.push_back(y) ;
            vertices.push_back(z) ;
        }
    }
    return vertices ;
}

int main()
{
    int success ;
    char infoLog[512] ;

    // Initialize GLFW
    if (!glfwInit()){
        std::cerr << "Failed to initialize GLFW" << std::endl;
        return -1;
    }

    // Customize Window Parameters
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 4);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 1);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
    glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE) ;

    // Create Window
    GLFWwindow* window = glfwCreateWindow(800, 600, "OpenGL Sphere", nullptr, nullptr);
    if (!window) {
        std::cerr << "Failed to create GLFW window" << std::endl;
        glfwTerminate();
        return -1;
    }
    glfwMakeContextCurrent(window);

    // Initialize GLEW
    glewExperimental = GL_TRUE;
    if (glewInit() != GLEW_OK) {
        std::cerr << "Failed to initialize GLEW" << std::endl;
        return -1;
    }

    // Compile Vertex Shader
    const char * vertex_shader_source = get_vertex_shader_source() ;
    GLuint vertexShader = glCreateShader(GL_VERTEX_SHADER) ;
    glShaderSource(vertexShader, 1, &vertex_shader_source, nullptr) ;
    glCompileShader(vertexShader) ;
    
    // Check for Compilation Errors
    glGetShaderiv(vertexShader, GL_COMPILE_STATUS, &success);
    if (!success){
        glGetShaderInfoLog(vertexShader, 512, NULL, infoLog);
        std::cerr << "ERROR::SHADER::VERTEX::COMPILATION_FAILED\n" << infoLog << std::endl;
    }

    // Compile Fragment Shader
    const char * fragment_shader_source = get_fragment_shader_source() ;
    GLuint fragmentShader = glCreateShader(GL_FRAGMENT_SHADER);
    glShaderSource(fragmentShader, 1, &fragment_shader_source, nullptr);
    glCompileShader(fragmentShader);

    // Check for Compilation Errors
    glGetShaderiv(fragmentShader, GL_COMPILE_STATUS, &success);
    if (!success){
        glGetShaderInfoLog(fragmentShader, 512, NULL, infoLog);
        std::cerr << "ERROR::SHADER::VERTEX::COMPILATION_FAILED\n" << infoLog << std::endl;
    }

    // Link the vertex and fragment shader into a shader program
    GLuint shaderProgram = glCreateProgram();
    glAttachShader(shaderProgram, vertexShader);
    glAttachShader(shaderProgram, fragmentShader);
    glBindFragDataLocation(shaderProgram, 0, "outColor");
    glLinkProgram(shaderProgram);
    glUseProgram(shaderProgram) ;

    // check for linking errors
    glGetProgramiv(shaderProgram, GL_LINK_STATUS, &success);
    if (!success) {
        glGetProgramInfoLog(shaderProgram, 512, NULL, infoLog);
        std::cerr << "ERROR::SHADER::PROGRAM::LINKING_FAILED\n" << infoLog << std::endl;
    }

    // Define Projection Matrix
    glm::mat4 projection = glm::perspective(glm::radians(45.0f), 800.0f / 600.0f, 0.1f, 100.0f) ;

    // Define View Matrix
    glm::mat4 view = glm::lookAt( glm::vec3(4,3,3), glm::vec3(0,0,0), glm::vec3(0,1,0) ) ;

    // Upload View and Projection Matrix
    glUniformMatrix4fv(glGetUniformLocation(shaderProgram, "projection"), 1, GL_FALSE, &projection[0][0]) ;
    glUniformMatrix4fv(glGetUniformLocation(shaderProgram, "view"), 1, GL_FALSE, &view[0][0]) ;

    // Create VAO
    GLuint vao;
    glGenVertexArrays(1, &vao);
    glBindVertexArray(vao);

    // Create VBO and EBO
    GLuint vbo, ebo;
    glGenBuffers(1, &vbo);
    glGenBuffers(1, &ebo);

    // Generate Sphere Vertices
    std::vector<float> vertices = generate_sphere_vertices(1.0f, 36, 18);

    // Generate Sphere Indices
    std::vector<unsigned int> indices = generate_sphere_indices(36, 18);

    // Store Vertex Data in VBO
    glBindBuffer(GL_ARRAY_BUFFER, vbo);
    glBufferData(GL_ARRAY_BUFFER, vertices.size() * sizeof(float), vertices.data(), GL_STATIC_DRAW);

    // Store Indices Data in EBO
    glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, ebo);
    glBufferData(GL_ELEMENT_ARRAY_BUFFER, indices.size() * sizeof(unsigned int), indices.data(), GL_STATIC_DRAW);

    // Specify the layout of the vertex data
    GLint posAttrib = glGetAttribLocation(shaderProgram, "position");
    glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
    glEnableVertexAttribArray(0) ;

    // Main Game Loop
    while (!glfwWindowShouldClose(window))
    {
        // Clear the screen to black
        glClearColor(0.0f, 0.0f, 0.0f, 1.0f);
        glClear(GL_COLOR_BUFFER_BIT);

        // Set Model Matrix
        float timeValue = glfwGetTime() ;
        float angle = timeValue * glm::radians(50.0f) ;
        glm::mat4 model = glm::rotate(glm::mat4(1.0f), angle, glm::vec3(0.0f, 1.0f, 0.0f)) ;
        glUniformMatrix4fv(glGetUniformLocation(shaderProgram, "model"), 1, GL_FALSE, glm::value_ptr(model)) ;

        // Draw a sphere
        glDrawElements(GL_TRIANGLES, indices.size(), GL_UNSIGNED_INT, 0);

        // Swap buffers and poll for and process events
        glfwSwapBuffers(window);
        glfwPollEvents();
    }

    // Delete Shaders
    glDeleteProgram(shaderProgram);
    glDeleteShader(fragmentShader);
    glDeleteShader(vertexShader);

    // Delete Buffers
    glDeleteBuffers(1, &vbo);
    glDeleteBuffers(1, &ebo);

    // Delete VAO
    glDeleteVertexArrays(1, &vao);

    // Terminat GLFW
    glfwTerminate() ;

    return 0;
}
```
<br>

If you run the code, you will get the following results: 

<br>
{% include figure.html path="assets/img/9.jpg" class="img-fluid rounded z-depth-1" %}
<br>

Thank you for reading !
